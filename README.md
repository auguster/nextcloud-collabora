# nextcloud-collabora
Nextcloud and Collabora require a reverse-proxy in order to work together.
Collabora requires a SSL connexion in order to work.
This can be a bit overwhelming to setup.

This project re-uses Nextcloud internal apache2 server as reverse-proxy for collabora.
It also removes collabora's SSL use as it's only accessible locally.

## How-to
### Collabora
Build a modified collabora/code docker image that removes SSL:
```
docker build -t collabora:nossl collabora/
```

Run collabora
```
docker run -d --name collabora -p 9980:9980 -e "domain=your\\.domain\\.here" -e "username=admin" -e "password=S3cRet" --cap-add MKNOD collabora:nossl
```
Go to `http://your.domain.here:9980` on your browser, it should display `ok` (it might also be loading forever which is also good). The server can take a while to start responding.
If you have still nothing try restarting the container:

```
docker stop collabora
docker start collabora
```

If Collabora refuses to work, you may be having trouble with the docker volume driver. I had to use overlay2 in order for it to work.

### Nextcloud
Build nextcloud with the reverse proxy configured for collabora:
```
docker build -t nextcloud nextcloud/
```

Run nextcloud:
```
docker run -d -p 80:80 nextcloud
```

Next you have to configure Nextcloud to tell it to use Collabora.
To do so, install the Collabora Application in Nextcloud.
Then, in "Administration -> Collabora Online" enter collabora's address and the `port 9980`.
It has to match one of the domain you used in Collabora's Docker run.

The Apache configuration assume Collabora and Nextcloud are on the same host.
`172.168.0.1` is the equivalent of localhost but for docker.

If you have a message telling you that your file is not compatible, you have to restart Collabora.

## NB
This is a weekend project, do not expect active development or support ;-)
