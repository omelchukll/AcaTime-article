# Runtime-only image — expects pre-built publish/ artifacts to be present
# Run build.ps1 (Windows) or build.sh (Linux/macOS) from the docker/ folder first
FROM mcr.microsoft.com/dotnet/aspnet:8.0

WORKDIR /app

# Install locale support, timezone data and fonts required by NPOI (Excel export)
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        locales \
        tzdata \
        fonts-liberation \
        fontconfig \
        curl && \
    sed -i '/uk_UA.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen uk_UA.UTF-8 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    fc-cache -f -v

RUN mkdir -p /app/logs && chown -R 1000:1000 /app/logs

# Copy pre-built publish output (populated by build.ps1 / build.sh)
COPY publish/ .

ENV ASPNETCORE_URLS=http://+:80
ENV LANG=uk_UA.UTF-8
ENV LC_ALL=uk_UA.UTF-8
ENV TZ=Europe/Kyiv
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false

ENTRYPOINT ["dotnet", "AcaTime.Web.Api.dll"]
