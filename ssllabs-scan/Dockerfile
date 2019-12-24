FROM golang

RUN git clone https://github.com/ssllabs/ssllabs-scan.git && cd ssllabs-scan && make

ENTRYPOINT ["/go/ssllabs-scan/ssllabs-scan-v3"]

