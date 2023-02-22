---

copyright:
  years: 2022, 2023
lastupdated: "2023-02-07"

keywords: confidential computing, secure execution, hpcr, contract, customization, env, workload, encryption, attestation, validating

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Validating the certificates
{: #cert_validate}

You can validate the certificates that you download for contract encryption and attestation.
{: shortdesc}

## Downloading the certificates
{: #download_cert}

Download the following certificates:
* Get the DigiCert certificates. The DigiCert Trusted Root G4 certificate can be downloaded [here](https://cacerts.digicert.com/DigiCertTrustedRootG4.crt.pem), and the Digicert G4 intermediate certificate can be downloaded [here](https://cacerts.digicert.com/DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem){: external}.
* Get the IBM intermediate certificate [here](/media/docs/downloads/hyper-protect-container-runtime/ibm-hyper-protect-container-runtime-1-0-s390x-8-intermediate.crt){: external}, for the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-8`.
    For the IBM Hyper Protect Container Runtime image version `ibm-hyper-protect-container-runtime-1-0-s390x-7`, you can download the IBM intermediate certificate [here](/media/docs/downloads/hyper-protect-container-runtime/ibm-hyper-protect-container-runtime-1-0-s390x-7-intermediate.crt){: external}.
    {: note}

## Validating the contract encryption certificate
{: #validate_encrypt_cert}

Complete the following steps on an Ubuntu system to validate the encryption certificate:

1. Use the following command to verify the CA certificate:
   ```sh
   openssl verify -crl_download -crl_check DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem
   ```
   {: pre}

2. Use the following command to verify the signing key certificate:
   ```sh
   openssl verify -crl_download -crl_check -untrusted DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem ibm-hyper-protect-container-runtime-1-0-s390x-8-intermediate.crt
   ```
   {: pre}

3. Complete the following steps to verify the signature of the encrypted certificate document:
   1. Extract the public signing key into a file. In the following example, the file is called `pubkey.pem`:
      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-8-intermediate.crt -pubkey -noout >  pubkey.pem
      ```
      {: pre}

   2. Extract the encryption key signature from the encryption certificate document.
      The following command returns the offset value of the signature:
      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-8-encrypt.crt | tail -1 | cut -d : -f 1
      ```
      Consider that the output of the command is `<offset_value>`. Use this `<offset_value>` to extract the encryption key signature into a file called signature:
      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-8-encrypt.crt -out signature -strparse <offset_value> -noout
      ```
      {: pre}

   3. Extract the body of the encryption certificate document into a file called body.
      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-8-encrypt.crt -out body -strparse 4 -noout
      ```
      {: pre}

   4. Verify the signature by using the signature and body files:
      ```sh
      openssl sha512 -verify pubkey.pem -signature signature body
      ```
      {: pre}

4. Verify the host key document issuer. Compare the output of the following two commands. The output should match.
   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-8-encrypt.crt  -issuer -noout
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-8-intermediate.crt -subject -noout
   ```
   {: pre}

5. Verify that the encryption certificate document is still valid by checking the output of the following command:
   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-8-encrypt.crt -dates -noout
   ```
   {: pre}

## Validating the attestation certificate
{: #validate_attest_cert}

Complete the following steps on an Ubuntu system to validate the attestation certificate:

1. Use the following command to verify the CA certificate:
   ```sh
   openssl verify -crl_download -crl_check DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem
   ```
   {: pre}

2. Use the following command to verify the signing key certificate:
   ```sh
   openssl verify -crl_download -crl_check -untrusted DigiCertTrustedG4CodeSigningRSA4096SHA3842021CA1.crt.pem ibm-hyper-protect-container-runtime-1-0-s390x-8-intermediate.crt
   ```
   {: pre}

3. Complete the following steps to verify the signature of the encrypted certificate document:
   1. Extract the public signing key into a file. In the following example, the file is called `pubkey.pem`:
      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-8-intermediate.crt -pubkey -noout >  pubkey.pem
      ```
      {: pre}

   2. Extract the attestation key signature from the attestation certificate document.
      The following command returns the offset value of the signature:
      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-8-attestation.crt | tail -1 | cut -d : -f 1
      ```
      Consider that the output of the command is `<offset_value>`. Use this `<offset_value>` to extract the attestation key signature into a file called signature:
      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-8-attestation.crt -out signature -strparse <offset_value> -noout
      ```
      {: pre}

   3. Extract the body of the attestation certificate document into a file called body.
      ```sh
      openssl asn1parse -in ibm-hyper-protect-container-runtime-1-0-s390x-8-attestation.crt -out body -strparse 4 -noout
       ```
      {: pre}

   4. Verify the signature by using the signature and body files:
      ```sh
      openssl sha512 -verify pubkey.pem -signature signature body
      ```
      {: pre}

4. Verify the host key document issuer. Compare the output of the following two commands. The output should match.
   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-8-attestation.crt -issuer -noout
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-8-intermediate.crt -subject -noout
   ```
   {: pre}

5. Verify that the attestation certificate document is still valid by checking the output of the following command:
   ```sh
   openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-8-attestation.crt -dates -noout
   ```
   {: pre}

## Certificate revocation list
{: #certificate-revocation-list}

Starting from `ibm-hyper-protect-container-runtime-1-0-s390x-7-encrypt.crt` and
`ibm-hyper-protect-container-runtime-1-0-s390x-7-attestation.crt`, the
certificates will contain **Certificate Revocation List (CRL) Distribution Points**. You can use the CRL to verify that your certificates are valid (not revoked).

1. Extract and download the CRL URL from the attestation or encryption certificate:
   ```sh
   openssl x509 -in "ibm-hyper-protect-container-runtime-1-0-s390x-8-encrypt.crt" -noout -ext crlDistributionPoints
   crl_url=https://ibm.biz/hyper-protect-container-runtime-023BC9-crl-1  # (example)
   curl --location --silent "$crl_url" --output "ibm-hyper-protect-container-runtime.crl"
   ```

2. Verify that the CRL is valid (check valid dates and issuer):
   ```sh
   openssl crl -text -noout -in "ibm-hyper-protect-container-runtime.crl"
   ```

3. Verify the CRL signature:
   ```sh
   openssl x509 -in "ibm-hyper-protect-container-runtime-1-0-s390x-8-intermediate.crt" -pubkey -noout -out pubkey
   bbegin="$(openssl asn1parse -in "ibm-hyper-protect-container-runtime.crl" | head -2 | tail -1 | cut -d : -f 1)"
   bend="$(openssl asn1parse -in "ibm-hyper-protect-container-runtime.crl" | tail -1 | cut -d : -f 1)"
   openssl asn1parse -in "ibm-hyper-protect-container-runtime.crl" -out signature -strparse $bend -noout
   openssl asn1parse -in "ibm-hyper-protect-container-runtime.crl" -out body -strparse $bbegin -noout
   openssl sha512 -verify pubkey -signature signature body
   ```

4. Verify that the encryption certificate document is valid: 
   1. Extract the serial from the encryption certificate:
      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-8-encrypt.crt -noout -serial
      serial=16B7C7F9B61548506F4E63BA6FD40045  # (example)
      ```
   2. Verify that the certificate is not listed within the CRL:
      ```sh
      openssl crl -text -noout -in "ibm-hyper-protect-container-runtime.crl" | grep -q "$serial" && echo REVOKED || echo OK
      ```
   A revoked encryption certificate document must not be used for further encryptions.
   
5. Verify that the attestation certificate document is valid: 
   1. Extract the serial from the attestation certificate:
      ```sh
      openssl x509 -in ibm-hyper-protect-container-runtime-1-0-s390x-8-attestation.crt -noout -serial
      serial=65BFC9AD7C3C269E41517E7FC26B0E3C  # (example)
      ```
   2. Verify that the certificate is not listed within the CRL:
      ```sh
      openssl crl -text -noout -in "ibm-hyper-protect-container-runtime.crl" | grep -q "$serial" && echo REVOKED || echo OK
      ```
   An image with a revoked attestation certificate document must not be started.
