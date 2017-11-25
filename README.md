# nextcloud-collabora
Nextcloud and Collabora require a reverse-proxy in order to work together.
Collabora requires a SSL connexion in order to work.
This can be a bit overwhelming to setup.

This project re-uses Nextcloud internal apache2 server as reverse-proxy for collabora.
It also removes collabora's SSL use as it's only accessible locally.

## NB
This is a weekend project, do not expect active development or support ;-)
