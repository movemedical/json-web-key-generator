# json-web-key-generator

A commandline Java-based generator for JSON Web Keys (JWK) and JSON Private/Shared Keys (JPSKs).

## Standalone run

To compile, run `mvn package`. This will generate a `json-web-key-generator-0.9-SNAPSHOT-jar-with-dependencies.jar` in the `/target` directory.

To generate a key, run `java -jar target/json-web-key-generator-0.9-SNAPSHOT-jar-with-dependencies.jar -t <keytype>`. Several other arguments are defined which may be required depending on your key type:

```
usage: java -jar json-web-key-generator.jar -t <keyType> [options]
 -t,--type <arg>           Key Type, one of: RSA, oct, EC, OKP
 -s,--size <arg>           Key Size in bits, required for RSA and oct key
                           types. Must be an integer divisible by 8
 -c,--curve <arg>          Key Curve, required for EC or OKP key type.
                           Must be one of P-256, secp256k1, P-384, P-521
                           for EC keys or one of Ed25519, Ed448, X25519,
                           X448 for OKP keys.
 -u,--usage <arg>          Usage, one of: enc, sig (optional)
 -a,--algorithm <arg>      Algorithm (optional)
 -i,--id <arg>             Key ID (optional), one will be generated if not
                           defined
 -g,--idGenerator <arg>    Key ID generation method (optional). Can be one
                           of: date, timestamp, sha256, sha1, none. If
                           omitted, generator method defaults to
                           'timestamp'.
 -I,--noGenerateId         <deprecated> Don't generate a Key ID.
                           (Deprecated, use '-g none' instead.)
 -p,--showPubKey           Display public key separately (if applicable)
 -S,--keySet               Wrap the generated key in a KeySet
 -o,--output <arg>         Write output to file. Will append to existing
                           KeySet if -S is used. Key material will not be
                           displayed to console.
 -P,--pubKeyOutput <arg>   Write public key to separate file. Will append
                           to existing KeySet if -S is used. Key material
                           will not be displayed to console. '-o/--output'
                           must be declared as well.
 -x,--x509                 Display keys in X509 PEM format
```

## Docker

### Build with docker
Example:


```bash
# Optional TAG
#TAG="your/tag:here"
# Example: TAG="<your_docker_id>/json-web-key-generator:latest"
$ docker build -t $TAG .
```

If building from git tags then run the following to store the *tag*, and the *commit*
in the docker image label.

```bash
TAG=$(git describe --abbrev=0 --tags)
REV=$(git log -1 --format=%h)
docker build -t <your_docker_id>/json-web-key-generator:$TAG --build-arg GIT_COMMIT=$REV --build-arg GIT_TAG=$TAG .
docker push <your_docker_id>/json-web-key-generator:$TAG

# or push all the tags
docker push <your_docker_id>/json-web-key-generator --all-tags
```

### Run from docker

Example of running the app within a docker container to generate a RSA (-t RSA) 2040 bit (-s 2048) signature use (-u sig) RS256 algo (-a RS256) SHA256 id generator (-g sha256) and print the results (-p)

An image is hosted on Movemedical's public ECR for convenience.

```bash
$ docker run --rm public.ecr.aws/n6n3r6m4/json-web-key-generator -t RSA -s 2048 -u sig -a RS256 -g sha256 -p
Full key:
{
  "p": "3Lgn0eDnrjhEC_tcKZJldFl4OLTyBaWAqtiozRqBJIg8vVPR2OY3q1yAQygWhllVnKKnXDZSrQAYaqMqRv24p0SxdJMcVaNES8fW0bZX7mEDXMCd4nwCgI_dibsl2ugq2uchFJRM6WtuwlruoKM9eEveCkfECjRh_C8rp7F-hSs",
  "kty": "RSA",
  "q": "0Dt5h_BswBUYWwu98h597imwoh7haGtxFJTD-xTN0C8P5j8jSzwwORhv8jo7YmyWstV3TR5M4XpMtRHoA0Cxir5afoaXOU1lWxA386ICMUjFx2bDjKx8Kx9_KBLF7g3YgReXAbMLfVq3NmVfOvx7zBCn7LZpmT8xZOOeAdCgqFc",
  "d": "mnU7PudGwJPRtI16ahKDX-vYjjljMH21MagWXcWoXvYwq_bzZ-Kpotv9XySFSTP9JDsKhyipHzGzWzw_7ZcrVfPAqjh-qe5k_dFuBD-ZJKx2syzswF6XNAVUc8CySNYSSEcVztJ8zxGL32-xn4IeyMbN1Fp8W6P_tt44YV8jd1NUOfpG8iObV5U9Ye8v62OIUcBM6eclNMDyUkpdBOTtps4G9RdwrvWmLsiCRqA4DE9GPi7DDXXS0ogl_36AAouhptAAwW_mmNnI5Wavbo-Zq5HUdcR1zyTbk9-I9spjRz1vhcPLJOc3MD3evtnwmuKGqrAU7a07dn7k7QEl7XHK5Q",
  "e": "AQAB",
  "use": "sig",
  "kid": "jwCtXNW2sa3sPmrdMPcIuaHG-morCAGVKTQgeqY2gnA",
  "qi": "xMQ5uzWPgkk6M-N4lB0CSQus0x-fO3aCoyDqnR4k6GsOGireJv-TrhVFYuoCI_XefBc03vemlGw9SaH9SgEpHSvcv_g6s70ILIhBw9zoy7jC1xuAaq45XsKBme36D_rGyJoTkaVc9ThS89g9KMNV96wIWQUhRBHs0xJewC2TN_k",
  "dp": "yeV6JzV_N5IoTH2E9FIBk8gzfEuoBxo49A5zegoAj5Y_WT_O-IS973YRrVyCHiqhcUInrOXUAoPP0dum1IFJ41emq2fVx1AtLNSD4BjXnioHlVRsF7wv3cG7eD1Eh1VPviUl0VlGcU3gZtAe77nihKOBXA4BeQQpjTDo0eA-Rzk",
  "alg": "RS256",
  "dq": "gWPjgY_o03aIOtLSBafi0mG_aw3LPMo-au6B1Pu5Y9pKg-TJto9A28mOjjKXAfK9tYQlbJseZKFNFtp4k8TYTYE41BQn1ah9CZfLXK1XtW4lz2DQtBHd2iHpLmpz6RdbZ-PTpm-t_QeofrmA8jM_ba8P2WwDtADrXWh-n1wW6GM",
  "n": "s4jnk_NUro_XI2gcVHSH4JO6Ut7kubSXjgraApHpAFMtkoxJIY7n-YYS5KJ5iRflxpn1YF2kWOnSoLNEtXKNp93-0cL0QYgQzOQSRzD7vOHj0h9Rtb5jvuleTdFzxd3ciDxS5dN5s-eopctdGxmvrP2xE0siAOHocs7BlzcL-aBa6uWZ90yHO_grQSYTcG45QC2hNy29DybNabIjOsXLLMaNL52wHbHGKAJjcmCBTg8CrzjHXJKpxZxZ0WWFnSzlilWtxoCC1ZXsMvVqqnXZKBCdryoqtdEor1OG28L-nOgt-YNBOrFjgcw7KXivTx53JqoKeFHUmGLag6zphEN5nQ"
}

Base64:
ewogICJwIjogIjNMZ24wZURucmpoRUNfdGNLWkpsZEZsNE9MVHlCYVdBcXRpb3pScUJKSWc4dlZQUjJPWTNxMXlBUXlnV2hsbFZuS0tuWERaU3JRQVlhcU1xUnYyNHAwU3hkSk1jVmFORVM4ZlcwYlpYN21FRFhNQ2Q0bndDZ0lfZGlic2wydWdxMnVjaEZKUk02V3R1d2xydW9LTTllRXZlQ2tmRUNqUmhfQzhycDdGLWhTcyIsCiAgImt0eSI6ICJSU0EiLAogICJxIjogIjBEdDVoX0Jzd0JVWVd3dTk4aDU5N2ltd29oN2hhR3R4RkpURC14VE4wQzhQNWo4alN6d3dPUmh2OGpvN1lteVdzdFYzVFI1TTRYcE10UkhvQTBDeGlyNWFmb2FYT1UxbFd4QTM4NklDTVVqRngyYkRqS3g4S3g5X0tCTEY3ZzNZZ1JlWEFiTUxmVnEzTm1WZk92eDd6QkNuN0xacG1UOHhaT09lQWRDZ3FGYyIsCiAgImQiOiAibW5VN1B1ZEd3SlBSdEkxNmFoS0RYLXZZampsak1IMjFNYWdXWGNXb1h2WXdxX2J6Wi1LcG90djlYeVNGU1RQOUpEc0toeWlwSHpHeld6d183WmNyVmZQQXFqaC1xZTVrX2RGdUJELVpKS3gyc3l6c3dGNlhOQVZVYzhDeVNOWVNTRWNWenRKOHp4R0wzMi14bjRJZXlNYk4xRnA4VzZQX3R0NDRZVjhqZDFOVU9mcEc4aU9iVjVVOVllOHY2Mk9JVWNCTTZlY2xOTUR5VWtwZEJPVHRwczRHOVJkd3J2V21Mc2lDUnFBNERFOUdQaTdERFhYUzBvZ2xfMzZBQW91aHB0QUF3V19tbU5uSTVXYXZiby1acTVIVWRjUjF6eVRiazktSTlzcGpSejF2aGNQTEpPYzNNRDNldnRud211S0dxckFVN2EwN2RuN2s3UUVsN1hISzVRIiwKICAiZSI6ICJBUUFCIiwKICAidXNlIjogInNpZyIsCiAgImtpZCI6ICJqd0N0WE5XMnNhM3NQbXJkTVBjSXVhSEctbW9yQ0FHVktUUWdlcVkyZ25BIiwKICAicWkiOiAieE1RNXV6V1Bna2s2TS1ONGxCMENTUXVzMHgtZk8zYUNveURxblI0azZHc09HaXJlSnYtVHJoVkZZdW9DSV9YZWZCYzAzdmVtbEd3OVNhSDlTZ0VwSFN2Y3ZfZzZzNzBJTEloQnc5em95N2pDMXh1QWFxNDVYc0tCbWUzNkRfckd5Sm9Ua2FWYzlUaFM4OWc5S01OVjk2d0lXUVVoUkJIczB4SmV3QzJUTl9rIiwKICAiZHAiOiAieWVWNkp6Vl9ONUlvVEgyRTlGSUJrOGd6ZkV1b0J4bzQ5QTV6ZWdvQWo1WV9XVF9PLUlTOTczWVJyVnlDSGlxaGNVSW5yT1hVQW9QUDBkdW0xSUZKNDFlbXEyZlZ4MUF0TE5TRDRCalhuaW9IbFZSc0Y3d3YzY0c3ZUQxRWgxVlB2aVVsMFZsR2NVM2dadEFlNzduaWhLT0JYQTRCZVFRcGpURG8wZUEtUnprIiwKICAiYWxnIjogIlJTMjU2IiwKICAiZHEiOiAiZ1dQamdZX28wM2FJT3RMU0JhZmkwbUdfYXczTFBNby1hdTZCMVB1NVk5cEtnLVRKdG85QTI4bU9qaktYQWZLOXRZUWxiSnNlWktGTkZ0cDRrOFRZVFlFNDFCUW4xYWg5Q1pmTFhLMVh0VzRsejJEUXRCSGQyaUhwTG1wejZSZGJaLVBUcG0tdF9RZW9mcm1BOGpNX2JhOFAyV3dEdEFEclhXaC1uMXdXNkdNIiwKICAibiI6ICJzNGpua19OVXJvX1hJMmdjVkhTSDRKTzZVdDdrdWJTWGpncmFBcEhwQUZNdGtveEpJWTduLVlZUzVLSjVpUmZseHBuMVlGMmtXT25Tb0xORXRYS05wOTMtMGNMMFFZZ1F6T1FTUnpEN3ZPSGowaDlSdGI1anZ1bGVUZEZ6eGQzY2lEeFM1ZE41cy1lb3BjdGRHeG12clAyeEUwc2lBT0hvY3M3Qmx6Y0wtYUJhNnVXWjkweUhPX2dyUVNZVGNHNDVRQzJoTnkyOUR5Yk5hYklqT3NYTExNYU5MNTJ3SGJIR0tBSmpjbUNCVGc4Q3J6akhYSktweFp4WjBXV0ZuU3psaWxXdHhvQ0MxWlhzTXZWcXFuWFpLQkNkcnlvcXRkRW9yMU9HMjhMLW5PZ3QtWU5CT3JGamdjdzdLWGl2VHg1M0pxb0tlRkhVbUdMYWc2enBoRU41blEiCn0=

Public key:
{
  "kty": "RSA",
  "e": "AQAB",
  "use": "sig",
  "kid": "jwCtXNW2sa3sPmrdMPcIuaHG-morCAGVKTQgeqY2gnA",
  "alg": "RS256",
  "n": "s4jnk_NUro_XI2gcVHSH4JO6Ut7kubSXjgraApHpAFMtkoxJIY7n-YYS5KJ5iRflxpn1YF2kWOnSoLNEtXKNp93-0cL0QYgQzOQSRzD7vOHj0h9Rtb5jvuleTdFzxd3ciDxS5dN5s-eopctdGxmvrP2xE0siAOHocs7BlzcL-aBa6uWZ90yHO_grQSYTcG45QC2hNy29DybNabIjOsXLLMaNL52wHbHGKAJjcmCBTg8CrzjHXJKpxZxZ0WWFnSzlilWtxoCC1ZXsMvVqqnXZKBCdryoqtdEor1OG28L-nOgt-YNBOrFjgcw7KXivTx53JqoKeFHUmGLag6zphEN5nQ"
}

Base64:
ewogICJrdHkiOiAiUlNBIiwKICAiZSI6ICJBUUFCIiwKICAidXNlIjogInNpZyIsCiAgImtpZCI6ICJqd0N0WE5XMnNhM3NQbXJkTVBjSXVhSEctbW9yQ0FHVktUUWdlcVkyZ25BIiwKICAiYWxnIjogIlJTMjU2IiwKICAibiI6ICJzNGpua19OVXJvX1hJMmdjVkhTSDRKTzZVdDdrdWJTWGpncmFBcEhwQUZNdGtveEpJWTduLVlZUzVLSjVpUmZseHBuMVlGMmtXT25Tb0xORXRYS05wOTMtMGNMMFFZZ1F6T1FTUnpEN3ZPSGowaDlSdGI1anZ1bGVUZEZ6eGQzY2lEeFM1ZE41cy1lb3BjdGRHeG12clAyeEUwc2lBT0hvY3M3Qmx6Y0wtYUJhNnVXWjkweUhPX2dyUVNZVGNHNDVRQzJoTnkyOUR5Yk5hYklqT3NYTExNYU5MNTJ3SGJIR0tBSmpjbUNCVGc4Q3J6akhYSktweFp4WjBXV0ZuU3psaWxXdHhvQ0MxWlhzTXZWcXFuWFpLQkNkcnlvcXRkRW9yMU9HMjhMLW5PZ3QtWU5CT3JGamdjdzdLWGl2VHg1M0pxb0tlRkhVbUdMYWc2enBoRU41blEiCn0=
```
