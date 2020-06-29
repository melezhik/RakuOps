
FROM jjmerelo/alpine-raku
RUN zef install --/test Sparrow6
RUN apk add bash perl
ADD sparrowfile .
COPY repo/ /root/repo/
RUN s6 --index-update
RUN raku -MSparrow6::DSL sparrowfile
