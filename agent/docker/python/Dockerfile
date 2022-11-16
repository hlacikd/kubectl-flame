FROM golang:1.14-buster as agentbuild
WORKDIR /go/src/github.com/VerizonMedia/kubectl-flame
ADD . /go/src/github.com/VerizonMedia/kubectl-flame
RUN go get -d -v ./...
RUN cd agent && go build -o /go/bin/agent

FROM python:3.8 AS pyspybuild
# install cargo
RUN apt-get update && apt-get install -y curl
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"
RUN pip3 install git+https://github.com/benfred/py-spy

FROM python:3.8
RUN mkdir /app
COPY --from=agentbuild /go/bin/agent /app/agent
COPY --from=pyspybuild /usr/local/bin/py-spy /app/py-spy

CMD [ "/app/agent" ]