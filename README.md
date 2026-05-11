# Apache Cassandra on Docker for Windows

This guide explains how to run Apache Cassandra locally on Windows using Docker.

## Prerequisites

- Docker Desktop installed and running
- WSL 2 enabled if you plan to use Bash from Windows Terminal
- Optional: `docker-compose` if you prefer using a compose file

## Quick Start

1. Pull the Cassandra image:

```powershell
docker pull cassandra:latest
```

2. Start Cassandra in a container:

```powershell
docker run --name my-cassandra -p 9042:9042 -d cassandra:latest
```

3. Follow the container logs until Cassandra is ready:

```powershell
docker logs my-cassandra -f
```

4. Connect to Cassandra with `cqlsh`:

```powershell
docker exec -it my-cassandra cqlsh
```

## Create a Keyspace and Table

Inside `cqlsh`, run:

```cql
CREATE KEYSPACE demo WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
USE demo;
CREATE TABLE users (
  id UUID PRIMARY KEY,
  name TEXT,
  email TEXT
);
INSERT INTO users (id, name, email) VALUES (uuid(), 'Saidul Hossain', 'shshohagh4@gmail.com');
SELECT * FROM users;
```

## Recommended Commands

- Stop the container:

```powershell
docker stop my-cassandra
```

- Start the container again:

```powershell
docker start my-cassandra
```

- Remove the container:

```powershell
docker rm my-cassandra
```

- Remove the image:

```powershell
docker rmi cassandra:latest
```

## Notes

- Cassandra listens on port `9042` by default.
- If you use `docker-compose`, you can map a host volume for data persistence.
- The sample `SELECT` query in this guide uses full table scanning by primary key; in production, use appropriate query patterns for Cassandra.

## Troubleshooting

- If `cqlsh` fails to connect, verify the container is running and `9042` is exposed.
- If the container is still starting, wait a few seconds and check logs again.
- On Windows, ensure Docker Desktop is using Linux containers.
