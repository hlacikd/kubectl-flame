FROM golang:1.14-buster as agentbuild
WORKDIR /go/src/github.com/VerizonMedia/kubectl-flame
ADD . /go/src/github.com/VerizonMedia/kubectl-flame
RUN go get -d -v ./...
RUN cd agent && go build -o /go/bin/agent

FROM python:3.8-alpine AS pyspybuild
# install cargo using alpine's package manager
RUN apk add --no-cache cargo git
RUN pip3 install git+https://github.com/benfred/py-spy

FROM python:3.8-alpine
RUN mkdir /app
COPY --from=agentbuild /go/bin/agent /app/agent
COPY --from=pyspybuild /usr/local/bin/py-spy /app/py-spy

CMD [ "/app/agent" ]