# Use the official Ubuntu image as the base image
FROM ubuntu:latest

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy all necessary files into the container
COPY . .

# Install necessary packages
RUN apt-get update && \
    apt-get install -y curl tar && \
    apt-get install -y openjdk-17-jdk

# Download the Besu binary
RUN curl -L https://github.com/hyperledger/besu/releases/download/24.3.3/besu-24.3.3.tar.gz -o besu.tar.gz

# Unpack the Besu binary
RUN tar -xzf besu.tar.gz && \
    mv besu-24.3.3/* . && \
    rmdir besu-24.3.3 && \
    rm besu.tar.gz

RUN chmod +x /usr/src/app/bin/besu

# Set environment variables and path
ENV PATH="/usr/src/app/bin:${PATH}"
ENV JAVA_TOOL_OPTIONS="-Dserver.port=8545 -Djava.net.preferIPv4Stack=true"

# Default command to run on container start
#     besu    --data-path=data    --genesis-file=./genesis.json    --rpc-http-enabled    --rpc-http-api=ETH,NET,QBFT    --host-allowlist="*"     --rpc-http-cors-origins="all"
CMD [\
        "besu",\
        "--data-path=data",\
        "--genesis-file=./genesis.json",\
        "--host-allowlist=*",\
        "--rpc-http-cors-origins=all",\
        "--rpc-http-enabled",\
        "--rpc-http-api=ETH,NET,QBFT",\
        "--rpc-http-host=0.0.0.0",\
        "--logging=DEBUG",\
        # to listen on all interfaces
        "--p2p-host=0.0.0.0",\
        "--p2p-interface=172.17.0.2"\
    ]

# Expose default port for JSON-RPC HTTP service
EXPOSE 8545
EXPOSE 30303