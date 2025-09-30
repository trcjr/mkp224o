FROM debian:latest AS builder

RUN apt-get update

RUN apt-get install -y \
    build-essential \
    gcc \
    g++ \
    musl-dev \
    autoconf \
    automake \
    libtool \
    pkg-config \
    git \
    wget \
    ca-certificates \
    libsodium-dev
WORKDIR /app
COPY . .
RUN ./autogen.sh
RUN ./configure CFLAGS="-O3 -mtune=generic -fomit-frame-pointer" LDFLAGS="-static"
RUN make
RUN strip mkp224o

FROM scratch
WORKDIR /app
COPY --from=builder /app/mkp224o .

ENTRYPOINT ["./mkp224o"]

# docker run -it -v $(pwd):/root/ mkp224o -S 5 -d /root/vanity-onions -n 1 $( LC_ALL=C tr -dc 'A-Z2-7' </dev/urandom | head -c 1; echo )

