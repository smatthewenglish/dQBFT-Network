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

# Default command to run on container start
#     besu    --data-path=data    --genesis-file=../genesis.json   --bootnodes=<Node-1 Enode URL>                                                                                                                                          --p2p-port=30304    --rpc-http-enabled    --rpc-http-api=ETH,NET,QBFT    --host-allowlist="*"  --rpc-http-cors-origins="all"  --rpc-http-port=8546
CMD ["besu", \
     "--data-path=data", \
     "--genesis-file=./genesis.json", \
    #             enode://fad3459a16de0b0d5aace774541848a40003b5f201991f6f5cd73c07fd34843faf6de32e84ddc6d2b3e1b7b8bcb48769a8dd48db70a91ad95d94fb3f5aa16ba7@0.0.0.0:30303"
     "--bootnodes=enode://fad3459a16de0b0d5aace774541848a40003b5f201991f6f5cd73c07fd34843faf6de32e84ddc6d2b3e1b7b8bcb48769a8dd48db70a91ad95d94fb3f5aa16ba7@172.17.0.2:30303", \
     "--p2p-port=30304", \
     "--rpc-http-enabled", \
     "--rpc-http-api=ETH,NET,QBFT", \
     "--host-allowlist=*", \
     "--rpc-http-cors-origins=all", \
     "--rpc-http-port=8546", \
     "--logging=DEBUG", \
     "--rpc-http-host=0.0.0.0", \
     "--p2p-interface=172.17.0.3",\
     "--p2p-host=0.0.0.0"]


# Expose default port for JSON-RPC HTTP service
EXPOSE 8546
EXPOSE 30304