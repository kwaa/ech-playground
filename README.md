# ECH Playground

Try using ECH (Encrypted Client Hello) for TLS-based proxies.

## HAProxy + NaiveProxy

I compiled OpenSSL and HAProxy as instructed in [esnistuff/haproxy.md](https://github.com/sftcd/openssl/blob/ECH-draft-13a/esnistuff/haproxy.md), and you can get the [x86_64 binaries](/haproxy/haproxy) and [Dockerfile](/haproxy/Dockerfile) from the `haproxy` folder of this repo.

Once I confirm that it works, I'll make the image available through GitHub Actions.

Meanwhile, the NaiveProxy client does not appear to support ECH at this time. ([naiveproxy#314](https://github.com/klzgrad/naiveproxy/issues/314))

### HAProxy Config

TODO

### NaiveProxy Config

Use the [same configuration](/naive/config.json) as [HAProxy Setup](https://github.com/klzgrad/naiveproxy/wiki/HAProxy-Setup).

```json
{
  "listen": "http://127.0.0.1:{{port}}",
  "padding": true
}
```

### Docker Compose

TODO

## Useful links

- [Developing ECH for OpenSSL (DEfO)](https://defo.ie/)
- [Experiences with implementing and deploying ECH](https://defo.ie/report.html)
- [sftcd/openssl-[ECH-draft-13a]](https://github.com/sftcd/openssl/tree/ECH-draft-13a)
- [sftcd/haproxy-[ECH-experimental]](https://github.com/sftcd/haproxy/tree/ECH-experimental)
- [esnistuff](https://github.com/sftcd/openssl/tree/ECH-draft-13a/esnistuff)
