`$ cat Dockerfile`

```dockerfile
FROM jjmerelo/alpine-raku
RUN zef install --/test Sparrow6
```


`$ docker build --tag rakuops:1.0 .`

```
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
```

`$ docker images`

```
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
rakuops                1.0                 a2cbc605ec5e        3 minutes ago       139MB
```


$ docker build --tag rakuops:1.0 .
```
Sending build context to Docker daemon  5.632kB
Step 1/4 : FROM jjmerelo/alpine-raku
 ---> c0ecb08ec5db
Step 2/4 : RUN zef install --/test Sparrow6
 ---> Using cache
 ---> a2cbc605ec5e
Step 3/4 : ADD sparrowfile .
 ---> 74c7ee71a303
Step 4/4 : RUN raku -MSparrow6::DSL sparrowfile
 ---> Running in c73e1a7d568f
unknown plugin bash
  in method plugin-install at /root/raku-install/share/perl6/site/sources/5D155994EC979DF8EF1FDED7148646312D9073E3 (Sparrow6::Task::Repository::Helpers::Plugin) line 115
  in sub task-run at /root/raku-install/share/perl6/site/sources/DB0BB8A1D70970E848E2F38D2FC0C39E4F904283 (Sparrow6::DSL::Common) line 12
  in sub bash at /root/raku-install/share/perl6/site/sources/7662EE0EFF4206F474B7CC4AEF229F1A86EC8FFF (Sparrow6::DSL::Bash) line 33
  in sub bash at /root/raku-install/share/perl6/site/sources/7662EE0EFF4206F474B7CC4AEF229F1A86EC8FFF (Sparrow6::DSL::Bash) line 7
  in block <unit> at sparrowfile line 1
```

# Create local sparrow repository

`$ s6 --repo-init local`

```
16:41:31 06/29/2020 [repository] repo initialization
16:41:31 06/29/2020 [repository] initialize Sparrow6 repository for /home/scheck/repo
```

```
$ git clone https://github.com/melezhik/sparrow-plugins`
$ cd sparrow-plugins/bash
$ s6 --upload
16:41:36 06/29/2020 [repository] upload plugin
16:41:36 06/29/2020 [repository] upload bash@0.2.1
```

`cp -r ~/repo .`

`cat Dockerfile`

```dockerfile
ADD sparrowfile .
# ...
RUN apk add bash perl
COPY repo/ /root/repo/
RUN s6 --index-update
RUN raku -MSparrow6::DSL sparrowfile
```

`$ docker build --tag rakuops:1.0 .`

```
Sending build context to Docker daemon  11.26kB
Step 1/7 : FROM jjmerelo/alpine-raku
 ---> c0ecb08ec5db
Step 2/7 : RUN zef install --/test Sparrow6
 ---> Using cache
 ---> a2cbc605ec5e
Step 3/7 : RUN apk add bash perl
 ---> Using cache
 ---> d9011d4e64db
Step 4/7 : ADD sparrowfile .
 ---> Using cache
 ---> adb1df57e1c0
Step 5/7 : COPY repo/ /root/repo/
 ---> Using cache
 ---> 3ed6bfaf4183
Step 6/7 : RUN s6 --index-update
 ---> Running in 6edfc480bde7
17:03:59 06/29/2020 [repository] update local index
17:03:59 06/29/2020 [repository] index updated from file:///root/repo/api/v1/index
Removing intermediate container 6edfc480bde7
 ---> 7eccb5889a80
Step 7/7 : RUN raku -MSparrow6::DSL sparrowfile
 ---> Running in af6eb4b2d9ee
17:04:02 06/29/2020 [repository] installing bash, version 0.002001
17:04:05 06/29/2020 [bash: echo Hello World] Hello World
```
