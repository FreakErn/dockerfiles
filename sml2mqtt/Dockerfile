FROM python:alpine

WORKDIR /

RUN pip install sml2mqtt

ENTRYPOINT [ "/usr/local/bin/sml2mqtt", "-c", "/sml2mqtt.conf" ]

