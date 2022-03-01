FROM docker:20.10.12-dind

RUN apk add bash icu-libs krb5-libs libgcc libintl libssl1.1 libstdc++ zlib
RUN apk add libgdiplus --repository https://dl-3.alpinelinux.org/alpine/edge/testing

# .NET Runtime version
ENV DOTNET_VERSION=6.0.100

# Install .NET Runtime
RUN wget -O dotnet.tar.gz https://dotnetcli.azureedge.net/dotnet/Sdk/$DOTNET_VERSION/dotnet-sdk-$DOTNET_VERSION-linux-musl-x64.tar.gz \
    && dotnet_sha512='428082c31fd588b12fd34aeae965a58bf1c26b0282184ae5267a85cdadc503f667c7c00e8641892c97fbd5ef26a38a605b683b45a0fef2da302ec7f921cf64fe' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -oxzf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
