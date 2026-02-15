# ipkit

Practical network utilities for DevOps and infrastructure teams. Simple, reliable, and automation-ready `ipkit.ir`.

## Databases (local only)

This service reads MaxMind MMDB files **from disk**. It does not download, embed, or fetch databases.

Default expected paths:

```
/var/lib/ipkit/db/
├── GeoLite2-Country.mmdb
├── GeoLite2-City.mmdb
└── GeoLite2-ASN.mmdb
```

## Configuration

- `IPKIT_MMDB_COUNTRY`: path to `GeoLite2-Country.mmdb` (default: `/var/lib/ipkit/db/GeoLite2-Country.mmdb`)
- `IPKIT_MMDB_CITY`: path to `GeoLite2-City.mmdb` (default: `/var/lib/ipkit/db/GeoLite2-City.mmdb`)
- `IPKIT_MMDB_ASN`: path to `GeoLite2-ASN.mmdb` (default: `/var/lib/ipkit/db/GeoLite2-ASN.mmdb`)
- `IPKIT_LISTEN_ADDR`: listen address (default: `:8080`)
- `IPKIT_CACHE_TTL`: cache TTL duration (default: `5m`, example: `300s`)
- `IPKIT_RATE_LIMIT_PER_MIN`: per-client-IP requests/minute (default: `120`)

## Run locally (go run)

Required: readable MMDB files on disk.

Example:

```bash
export IPKIT_MMDB_COUNTRY=/var/lib/ipkit/db/GeoLite2-Country.mmdb
export IPKIT_MMDB_CITY=/var/lib/ipkit/db/GeoLite2-City.mmdb
export IPKIT_MMDB_ASN=/var/lib/ipkit/db/GeoLite2-ASN.mmdb
export IPKIT_LISTEN_ADDR=":8080"

go run ./cmd/ipkit
```

## Build a binary

```bash
go build -o ipkit ./cmd/ipkit
```

## Run the binary

```bash
export IPKIT_MMDB_COUNTRY=/var/lib/ipkit/db/GeoLite2-Country.mmdb
export IPKIT_MMDB_CITY=/var/lib/ipkit/db/GeoLite2-City.mmdb
export IPKIT_MMDB_ASN=/var/lib/ipkit/db/GeoLite2-ASN.mmdb
export IPKIT_LISTEN_ADDR=":8080"

./ipkit
```

## Systemd

1) Install the binary:

```bash
sudo install -m 0755 ./ipkit /usr/local/bin/ipkit
```

2) Install the unit file:

```bash
sudo cp ./ipkit.service /etc/systemd/system/ipkit.service
sudo systemctl daemon-reload
sudo systemctl enable ipkit
sudo systemctl start ipkit
```

