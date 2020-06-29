$ cat Dockerfile

```dockerfile
FROM jjmerelo/alpine-raku
RUN zef install --/test Sparrow6
```

$ docker build --tag rakuops:1.0 .
Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM jjmerelo/alpine-raku
latest: Pulling from jjmerelo/alpine-raku
df20fa9351a1: Already exists
a901eee946d8: Pull complete
Digest: sha256:3e22846977d60ccbe2d06a47da4a5e78c6aca7af395d57873d3a907bea811838
Status: Downloaded newer image for jjmerelo/alpine-raku:latest
 ---> c0ecb08ec5db
Step 2/2 : RUN zef install --/test Sparrow6
 ---> Running in ae2a0dc8848f
===> Searching for: Sparrow6
===> Updating cpan mirror: https://raw.githubusercontent.com/ugexe/Perl6-ecosystems/master/cpan1.json
===> Searching for missing dependencies: File::Directory::Tree, Hash::Merge, YAMLish, JSON::Tiny, Data::Dump
===> Searching for missing dependencies: MIME::Base64
===> Installing: File::Directory::Tree:auth<labster>
===> Installing: Hash::Merge:ver<1.0.1>:auth<github:scriptkitties>:api<1>
===> Installing: MIME::Base64:ver<1.2.1>:auth<github:retupmoca>
===> Installing: YAMLish:ver<0.0.5>
===> Installing: JSON::Tiny:ver<1.0>
===> Installing: Data::Dump:ver<v.0.0.11>:auth<github:tony-o>
===> Installing: Sparrow6:ver<0.0.24>

1 bin/ script [s6] installed to:
/root/raku-install/share/perl6/site/bin
===> Updated cpan mirror: https://raw.githubusercontent.com/ugexe/Perl6-ecosystems/master/cpan1.json
===> Updating p6c mirror: https://raw.githubusercontent.com/ugexe/Perl6-ecosystems/master/p6c1.json
===> Updated p6c mirror: https://raw.githubusercontent.com/ugexe/Perl6-ecosystems/master/p6c1.json
Removing intermediate container ae2a0dc8848f
 ---> a2cbc605ec5e
Successfully built a2cbc605ec5e
Successfully tagged rakuops:1.0