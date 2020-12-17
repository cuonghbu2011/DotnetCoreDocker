FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS builder

WORKDIR /app

# copy csproj and restore as distinct layers
COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS runtime
WORKDIR /app
COPY --from=builder /app/out .

RUN chgrp -R 0 /app && \
    chmod -R g=u /app

# USER 1001
# RUN chown -R 1001:0 /app

RUN chmod 775 /app

ENTRYPOINT ["dotnet", "OpenShiftIntergate.dll", "--urls", "http://*:8080"]