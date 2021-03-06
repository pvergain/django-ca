###########
Development
###########

**********
Setup demo
**********

You can set up a demo using ``fab init_demo``. First create a minimal
``localsettings.py`` file (in ``ca/ca/localsettings.py``)::

   DEBUG = True
   SECRET_KEY = "whatever"

And then simply run ``fab init_demo`` from the root directory of your project.

**************
Run test-suite
**************

To run the test-suite, simply execute::

   python setup.py test

... or just run some of the tests::

   python setup.py test --suite=tests_command_dump_crl

To generate a coverate report::

   python setup.py coverage

***********************
Useful OpenSSL commands
***********************

.. highlight:: none

CRLs
====

Convert a CRL to text on stdout::

   openssl crl -inform der -in sfsca.crl -noout -text

Convert a CRL to PEM to a file::

   openssl crl -inform der -in sfsca.crl -outform pem -out test.pem

Verify a certificate using a CRL::

   openssl verify -CAfile files/ca_crl.pem -crl_check cert.pem

OCSP
====

Run a OCSP responder::

   openssl ocsp -index files/ocsp_index.txt -port 8888 \
      -rsigner files/localhost.pem -rkey files/localhost.key \
      -CA ca.pem -text

Verify a certificate using OCSP::

  openssl ocsp -CAfile ca.pem -issuer ca.pem -cert cert.pem \
      -url http://localhost:8888 -resp_text

Other
=====

Convert a p7c/pkcs7 file to PEM (Let's Encrypt CA Issuer field) (see also
:manpage:`pkcs7(1SSL)` -
`online <https://www.openssl.org/docs/manmaster/apps/pkcs7.html>`_)::

   openssl pkcs7 -inform der -in letsencrypt.p7c -print_certs \
      -outform pem -out letsencrypt.pem

*****************************
Development webserver via SSL
*****************************

To test a certificate in your webserver, first install the root certificate
authority in your browser, then run ``stunnel4`` and ``manage.py runserver`` in
two separate shells::

   stunnel4
   HTTPS=1 python manage.py runserver 8001

Then visit https://localhost:8443.
