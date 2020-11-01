Gitolite transparency log
=========================
All git-receive operations are logged in the transparency log, published
at https://git.kernel.org/pub/scm/infra/transparency-logs/gitolite/git/.
The repository in in the public-inbox v2 format and each operation is
recorded as a separate RFC822 message in the YAML format.

* https://public-inbox.org/public-inbox-v2-format.html

Others are invited to clone this repository and update it periodically
in order to preserve a distributed tamper-evident log of all write
activity to the kernel.org source repositories.

Sample record
-------------
Below is the annotated sample record. It can be viewed in the log at the
following URL:

* https://git.kernel.org/pub/scm/infra/transparency-logs/gitolite/git/1.git/plain/m?id=aca1687845b64383ec52379c86b10eaa9865c1fa

::

    Content-Type: multipart/mixed; boundary="===============9216280479104659071=="
    MIME-Version: 1.0
    From: Gitolite Activity Feed <devnull@kernel.org>
    Subject: post-receive: pub/scm/linux/kernel/git/mricon/hook-test
    Date: Sun, 01 Nov 2020 14:30:04 -0000

The Date field is the time when gitolite received the push, not the time
of any of the commits.

::

    Message-Id: <160424100444.28613.2392504408513983803@gitolite.kernel.org>

    --===============9216280479104659071==
    Content-Type: text/plain; charset="us-ascii"
    MIME-Version: 1.0
    Content-Transfer-Encoding: 7bit

If there are any attachments, the message will be MIME-formatted,
otherwise it will be a text/plain message.

::

    ---
    service: git-receive-pack
    repo: pub/scm/linux/kernel/git/mricon/hook-test
    user: mricon
    remote_ip: xHVq6qQJwVPokJmgTq0F/d+8fco=

The ``remote_ip`` field is calculated the following way:
``base64(sha1("{secret}{username}{actual_remote_ip}"))``. The ``{secret}`` is
a 32-character long alphanumeric string and is rotated daily at 00:00
UTC. All rotated secrets are logged internally and can be used for
forensic purposes at a later date.

This scheme was chosen to preserve user privacy, but provide a way to
identify when pushes were received from different sources within the
boundaries of the same calendar day.

::

    git_push_cert_status: G

If the push was signed, the ``git_push_cert_status`` field will be
present and the push certificate will be attached as a separate file
(see further).

::

    changes:
      - ref: refs/heads/main
        old: 29000644713a1a7ecea7871b433cf83f2740da90
        new: fe6ba5369e98005e5f0d76442447a966aa70d0dc
        log: |
             fe6ba5369e98005e5f0d76442447a966aa70d0dc Another push to test transparency log

The ``changes`` field is an array of values per each of the refs pushed
during the single git-receive-pack invocation. The ``log`` field is the
enumeration of commits from the previous ref to the new ref. If it is
less than 1024KB in size, the contents will be listed in the YAML body
itself. If larger, they will be attached as a separate file, with the
name of the attached file listed instead.

::

    --===============9216280479104659071==
    Content-Type: text/plain; charset="us-ascii"
    MIME-Version: 1.0
    Content-Transfer-Encoding: 7bit
    Content-Disposition: attachment; filename=git-push-certificate.txt

    certificate version 0.1
    pusher B6C41CE35664996C! 1604241004 -0500
    pushee gitolite.kernel.org:pub/scm/linux/kernel/git/mricon/hook-test
    nonce 1604241004-62dd093c179451c97bdae81a969305afa7e5d0c3

    29000644713a1a7ecea7871b433cf83f2740da90 fe6ba5369e98005e5f0d76442447a966aa70d0dc refs/heads/main
    -----BEGIN PGP SIGNATURE-----

    iHUEABYIAB0WIQR2vl2yUnHhSB5njDW2xBzjVmSZbAUCX57GbAAKCRC2xBzjVmSZ
    bDqdAQDvt+AXjrWKuEHRf9fh4+2UyyuGApeS7vS8wl8FoZZN4QEApgZeaokAJFQG
    1QG+be1u+RFC6CDIEh0vx/nThhaZDgI=
    =Bpxo
    -----END PGP SIGNATURE-----

    --===============9216280479104659071==--

If a push certificate is present, it will be attached to the body of the
message.

Signing your pushes
-------------------
Since several members of the Linux Foundation IT team have direct
backend access to the gitolite server, any one of them (or anyone in
possession of their compromised account) can fake a push record. If you
would like to help hedge against this risk, you are invited to sign your
pushes.

You can enable push signing by adding the following to your
``.git/config`` (or ``~/.gitconfig``)::

    [push]
        gpgSign = if-asked

See ``git-push`` for more information on this feature:

* https://git-scm.com/docs/git-push#Documentation/git-push.txt---signedtruefalseif-asked

Note: we only add the certificates to the transparency log at this time.
