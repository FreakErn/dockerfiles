from node:6

MAINTAINER  FreakErn git@freakern.de

RUN [ "cross-build-start" ]

RUN npm install ps4-waker -g

RUN [ "cross-build-end" ]  

ENTRYPOINT ["/usr/local/bin/ps4-waker"]
CMD ["--version"] 
