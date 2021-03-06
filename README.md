# SSL Certificate Chain Resolver
[![Latest Version](https://img.shields.io/github/release/freekmurze/ssl-certificate-chain-resolver.svg?style=flat-square)](https://github.com/freekmurze/ssl-certificate-chain-resolver/releases)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/freekmurze/ssl-certificate-chain-resolver/master.svg?style=flat-square)](https://travis-ci.org/freekmurze/ssl-certificate-chain-resolver)
[![SensioLabsInsight](https://img.shields.io/sensiolabs/i/2912a3ab-51a8-4e07-9bad-fd94a833f989.svg)](https://insight.sensiolabs.com/projects/2912a3ab-51a8-4e07-9bad-fd94a833f989)
[![Quality Score](https://img.shields.io/scrutinizer/g/freekmurze/ssl-certificate-chain-resolver.svg?style=flat-square)](https://scrutinizer-ci.com/g/freekmurze/ssl-certificate-chain-resolver)
[![Total Downloads](https://img.shields.io/packagist/dt/spatie/ssl-certificate-chain-resolver.svg?style=flat-square)](https://packagist.org/packages/spatie/ssl-certificate-chain-resolver)

All operating systems contain a set of default trusted root certificates. But Certificate Authorities usually don't use their root certificate to sign customer certificates. They use so called intermediate certificates instead, because these can be rotated more frequently.

If not all intermediate certificates are installed on your server, some clients —mostly mobile browsers— will think you are on an insecure connection.

This tool can help you fix the [incomplete certificate chain issue](#background-the-trust-chain), also reported as *Extra download* by [Qualys SSL Server Test](https://www.ssllabs.com/ssltest/).

If you need an online tool to solve this issue, take a look at [certificatechain.io](https://certificatechain.io)

## Installation

This package can be installed using composer by running this command.

```bash
composer global require spatie/ssl-certificate-chain-resolver
```

## Usage

Let's assume you have an incomplete certificate  called ```cert.crt```. To generate the a file containing the certificate and the entire trust chain, you can use this command:

```bash
ssl-certificate-chain-resolver resolve cert.crt
```

A file containing the certificate and the entire trust chain will be saved as ```certificate-including-trust-chain.crt```

You can also pass the name of the file of the outputfile as the second argument:
```bash
ssl-certificate-chain-resolver resolve cert.crt your-output-file.crt
```

If the outputfile already exists, you will be asked if it's ok to overwrite it.

## Updating

You can update <b>ssl-certificate-chain-resolver</b> to the latest version by running:

```bash
composer global update spatie/ssl-certificate-chain-resolver
```

## Testing

Functional and unit tests are included with the package.

You can run them yourself with ```vendor/bin/codecept run```


## Credits

- [Freek Van der Herten](https:/murze.be)
- [All Contributors](https://github.com/freekmurze/ssl-certificate-chain-resolver/contributors)

This package was inspired by [cert-chain-resolver](https://github.com/zakjan/cert-chain-resolver/) written by [Jan Žák](http://www.zakjan.cz/). Some text, mainly the background about the trust chain, was copied from the readme of his repo.


## Background: the trust chain

All operating systems contain a set of default trusted root certificates. But Certificate Authorities usually don't use their root certificate to sign customer certificates. Instead of they use so called intermediate certificates, because they can be rotated more frequently.

A certificate can contain a special Authority Information Access extension (RFC-3280) with URL to issuer's certificate. Most browsers can use the AIA extension to download missing intermediate certificate to complete the certificate chain. This is the exact meaning of the Extra download message. But some clients, mostly mobile browsers, don't support this extension, so they report such certificate as untrusted.

This results in 'untrusted'-warnings like this, since the browser thinks you are on an insecure connection.

![Untrusted Warning](images/untrusted.png)

A server should always send a complete chain, which means concatenated all certificates from the certificate to the trusted root certificate (exclusive, in this order), to prevent such issues.  So when installing a SSL certificate on a server you should install all intermediate certificates as wel. You should be able to fetch intermediate certificates from the issuer and concat them together by yourself.

This tool helps you automatize that boring task by looping over certificate's AIA extension field.

## License

The MIT License (MIT). Please see [License File](LICENSE) for more information.


