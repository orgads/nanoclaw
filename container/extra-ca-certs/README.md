# Extra CA certificates

Drop `.crt` files here (PEM-encoded, `.crt` extension required) to have them
baked into the agent image's trust store at build time.

Needed when the network does TLS interception (corporate MITM proxy): the
build downloads Bun, npm/pnpm packages and CLIs over HTTPS, and those fail
with `curl: (60) SSL certificate problem: self-signed certificate in
certificate chain` unless the intercepting CA is trusted inside the container.

On Debian/Ubuntu hosts the CA you need is usually already in
`/usr/local/share/ca-certificates/`. Copy it in, then rebuild:

    cp /usr/local/share/ca-certificates/<your-ca>.crt container/extra-ca-certs/
    ./container/build.sh

This directory is empty by default and the build is a no-op without it.
