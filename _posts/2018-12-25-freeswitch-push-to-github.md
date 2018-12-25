---
layout: post
title: "Freeswitch push to github"
date: 2018-12-25 15:44:36 +0800
categories: notes
tags: freeswitch
comments: 1
---

When push freeswitch git to github had some error,this problem  also found by otherbody 
report a bug in freeswitch jira :https://freeswitch.org/jira/browse/FS-11338

the error is :
``` 
git fsck 
Checking object directories: 100% (256/256), done. 
error in commit 487128950df6ee433c131b5feaafe81ee86629f4: multipleAuthors: invalid format - multiple 'author' lines 
error in commit 8574988c3a378b4d5861ecaeb0e958657635703b: multipleAuthors: invalid format - multiple 'author' lines 
Checking objects: 100% (307936/307936), done. 
dangling blob ca8e65ad0ac3b884f4f5f8c8b0aecb576a9c42e0 
``` 

this problem only can be fix by rebase ,but the gitobject need change
so I had to fork myself branch to fix this problem.

the command is:
```
## build master_new
# found error gitobject
 git fsck  
#c onfig author
 git config --global user.email "liuzhuoren@utry.cn"
 git config --global user.name "liuzhuoren"
# begin rebase to fix error gitobject "multipleAuthors"
 git rebase -i 8574988c3a378b4d5861ecaeb0e958657635703b~1
 git commit --amend --reset-author
 git rebase --continue
 git commit --allow-empty
 git rebase --continue
 
## build v1.6_new
git clean -df
git status
git branch
git rebase 16d600c0350a79c2532c739dd1432f7ed318ea09^ 43a9feb7f8705af5008c963e36e36b9b1d69c373 --onto v1.6_new
git status
git rm build/startup/freeswitch.service.in build/startup/install_systemd.sh.in
git status
git add build/Makefile.am configure.ac
git rebase --continue
git status
git rebase --continue
git status
git rebase --skip
git status
git rm build/startup/freeswitch.default  build/startup/freeswitch.tmpfile
git rebase --skip
git branch
git log
git branch
git checkout v1.6_new
git reset --hard b0c0255facd0ea0f833f93e4a69c996ccb6cb305

## build v1.8_new
git log
git branch
git status
git rebase f7e2505fc7b007f29b4c61de1cf2337b07e3b589^ 4d4c454d3ef694767fbcefb3e444d23301dbbcab --onto v1.8_new
git branch
git checkout v1.8_new
git branch
git reset --hard 90c2a2afbd
git branch
git checkout master
git push local master
git status
git push local master
git push local v1.8_new
git push local v1.6_new
git push local 

## build v1.4_new
git log
git branch
git status
git rebase 6c5a17894c32df67955c816fd29d89bc8665042e^ ca9207aa32ca70b5d93f77390ca78fde2ea9c928 --onto v1.4_new
git checkout v1.4_new
git reset --hard  81ad2d5cab

## push to github
git push github master_new
git push github v1.2
git push github v1.4_new
git push github v1.6_new
git push github v1.8_new

```


the master_new branch fork log:
```

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git branch -a
* master
  remotes/origin/FI-393-change-fs_cli-banner-to-annual-conf
  remotes/origin/FS-10145-update-mod_v8-javascript-engine
  remotes/origin/FS-11353
  remotes/origin/FS-11393-record_trimmed_ms
  remotes/origin/FS-11421
  remotes/origin/FS-8750
  remotes/origin/FS-8971
  remotes/origin/FS-9785-debian-testing-now-relies-on-openssl
  remotes/origin/FS-9819-add-support-to-input-format-f-video
  remotes/origin/HEAD -> origin/master
  remotes/origin/adapterjs
  remotes/origin/apr-new
  remotes/origin/arranged-bridge
  remotes/origin/broadcast-early-media
  remotes/origin/bugfix/FS-10028-refactor-verto-lib-to-remove-jquery
  remotes/origin/bugfix/FS-10070-mod_nibblebill-overbilling-issue
  remotes/origin/bugfix/FS-10080-change-img-format-argb-to-match
  remotes/origin/bugfix/FS-10089-combining-inherit_codec-and-ice
  remotes/origin/bugfix/FS-10135-support-broken-streams-never-send
  remotes/origin/bugfix/FS-10716-fs-sofia_reg_check_ping_expire-is
  remotes/origin/bugfix/FS-7851-crash-when-the-non-moderator-participants
  remotes/origin/bugfix/FS-8137
  remotes/origin/bugfix/FS-8645-sync-ui-when-layout-or-res-id-changes
  remotes/origin/bugfix/FS-8785-enhance-ice-support-to-be-more-vanilla
  remotes/origin/bugfix/FS-8849-fs-is-not-authicate-messages-when
  remotes/origin/bugfix/FS-8865-mod-format-json-locking
  remotes/origin/bugfix/FS-9053-segfault-due-to-missing-received
  remotes/origin/bugfix/FS-9144-implement-video-mute-exit-canvas
  remotes/origin/bugfix/FS-9268-canot-hangup-the-call-as-the-bye
  remotes/origin/bugfix/FS-9592-mod_httapi-doesn-t-reset-one_time_params
  remotes/origin/bugfix/FS-9691-hung-in-mutex-when-accessing-sql
  remotes/origin/bugfix/FS-9854-mod_sofia-sdp-o-a-fails-to-put-sdp
  remotes/origin/bugfix/FS-9903-add-msrp-client-side-support
  remotes/origin/bugfix/FS-9904-msrp-refactor
  remotes/origin/conference-fixes-and-improvements
  remotes/origin/core_pgsql
  remotes/origin/dingaling_video
  remotes/origin/dispatcher
  remotes/origin/distort
  remotes/origin/feature/FS-10463-refactor-mod_av-to-support-ffmpeg
  remotes/origin/feature/FS-10557-mod_pcap
  remotes/origin/feature/FS-10753-fs-pre-2.0
  remotes/origin/feature/FS-8168-optimize-video-to-more-efficiently
  remotes/origin/feature/FS-8649-freeswitch-automated-call-scenario
  remotes/origin/feature/FS-8712-growl-type-notifications-for-admins
  remotes/origin/feature/FS-8800-amqp-commands-queue-routing-key-variable
  remotes/origin/feature/FS-8817-add-a-way-to-subscribe-filter-to
  remotes/origin/feature/FS-8828-create-sip-endpoint-using-libre-as
  remotes/origin/feature/FS-8977-add-support-for-nvenc-h264
  remotes/origin/feature/FS-9242-update-webrtc-code-in-verto-to-match
  remotes/origin/feature/FS-9340-add-1.9-branch
  remotes/origin/feature/FS-9340-add-1.9-branch-3
  remotes/origin/feature/FS-9555-mod_aes_wav-format-module-to-play
  remotes/origin/feature/bkw-request-skinny-contact
  remotes/origin/fs-42-fix
  remotes/origin/gsm_improvements
  remotes/origin/h263
  remotes/origin/invite_require_matching_reg
  remotes/origin/lede
  remotes/origin/libks
  remotes/origin/libyuv
  remotes/origin/master
  remotes/origin/new_deb_rules
  remotes/origin/ooh323
  remotes/origin/parsed_originate
  remotes/origin/pjsip_compat_fix
  remotes/origin/sangoma_1.6_raw
  remotes/origin/staging-1.9
  remotes/origin/test-non-recursive
  remotes/origin/tony
  remotes/origin/v1.2
  remotes/origin/v1.2.stable
  remotes/origin/v1.4
  remotes/origin/v1.4.beta
  remotes/origin/v1.6
  remotes/origin/v1.8
  remotes/origin/verto-stats
  remotes/origin/video-media-bug


A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git fsck
Checking object directories: 100% (256/256), done.
error in commit 487128950df6ee433c131b5feaafe81ee86629f4: multipleAuthors: invalid format - multiple 'author' lines
error in commit 8574988c3a378b4d5861ecaeb0e958657635703b: multipleAuthors: invalid format - multiple 'author' lines
Checking objects: 100% (314215/314215), done.
error: refs/remotes/origin/HEAD: invalid sha1 pointer 0000000000000000000000000000000000000000
dangling commit 2e9100f4f700974979312cd6741c65b32dbd2b99
dangling commit bec1d0caf77c08c36f22eb6598d27ce5906739c4
dangling commit 4022315e94eec08e93b2d0f4e10e007de3bb8a44
dangling commit 792d7112e5ee0f831a2f1e753e7ad23f353120b0
dangling commit 7e27729030501984c9cea74d3620b736df90d601
dangling commit fd3692da5654f4d4f92dd9798a2906cd424b08c4
dangling commit 8851d211358c3904cc4027ebd2648909aacf8f00
dangling commit 3d8fa2683879daeeaf1be0f1d1eeaed2cf19c841
dangling commit 45a5720e2e593b8da4b7ec06718221ec2038dbef
dangling commit 09d422ad325e79b727297a639e5d78236cba6ed1
dangling commit 7f01f3454ca7f87ad89eeace1c494cd88148ba4f
dangling commit c90303f63fc37337193a99b51f43f68f54d684ec
dangling commit 0734737dcb1acdb7aaebe59e210065bc56650e42
dangling commit 506a038616c7fef8072971e01297ce918630a577
dangling commit 3c9e23ef0038c5a3b1ca2a87ed24e08b980bc218
dangling commit 42ce33238fae30b8e5b39cd2d79080416d069130
dangling commit 69dd036a3553d9e9d9018ff59506c92b5785bf77
dangling commit d1e1b39f9808f052f5d88a7cc33c25812f9ad59d
dangling commit 5beb33895675cc909c1bd3bc38a0cf58a2e4eb1c
dangling commit 42fdd3865739f8fc5cfbfff474f6d6576c0b76b9
dangling commit 49047462feddb4af93c829c69c1a5e10891bf866
dangling commit 4038341ff9407a2f71ed24032be5fc355ebff6db
dangling commit 684524fa26a856665ff09cbf27d141e910cd60b8
dangling commit ec521413693611e2baa190e561d00498acdbe0c7
dangling commit 06e6c4e3a37118efbd248222aa4776de3b162858
dangling commit 41e6144871dd102c8e1e809deb46a93adace7655
dangling commit 2477f5058a69c48824b794445f1727e9189b80d2
dangling commit 3cc0350c3a4258225295ed7b4a7f7c990339df9f
dangling commit f23ef68ef4dcd94a5848cb8c4ec395c26afc4afc
dangling commit 614736ede68fcbd478ce3117668c99a4c50fb3f1
dangling commit 627896812d0aee97b6cd823c41fd9b9ae706e3f3
dangling commit d08aa6eb22617907c54d48c452c8708bd09947ce
dangling commit 7ec976de51641a17cba3ad6dc9e61e68bb45c519
dangling commit 0ee2d6637d53feed758cd759f43908bb250a4f95
dangling commit ca1557749e22f722578446a9d0783985c44da277
dangling commit f6179720e6be3ce6ed9dc60efb4144ae69a79e17
dangling commit 6d2d07b29d9aebcd83171fa47b3000d56e003a47
dangling commit f834377483584d9444cfcf587a9935964ecb427c
dangling commit b0613791a9e0ba35efc06eb6b16d4ea7a4567b08
dangling commit ca9207aa32ca70b5d93f77390ca78fde2ea9c928
dangling commit 129cd739a406c7c142ca9e87a2e6b34a3f61f94a
dangling commit 91f9d7182e71f6b474522487389cf6e64dbed822
dangling commit 6c08785008ef24b32a67ae93b572675a87af98f4
dangling commit a51518f75330853a940dcec3e7531b7a0a5b687b
dangling commit cc2308c2895b47f183fbb58c62d20422493dbc15
dangling commit b0074957a3918955a37f6e059e3e4e37b2595115
dangling commit 8b2bb9dddea10fed52144526e93075361a832834
dangling commit e93ae94f68fd9212a6ff196e71f0c5a9162fb7b5
dangling commit a8f5493fea6756def215115cf26194a05c94a838
dangling commit da04da87df376009929ced181c130b62fad64860
dangling commit d1aa8a9db297e647208920f659d09f8070447445
dangling commit 6fe55af6b849495b7e36a64ac4898cfaac135660
dangling commit 46263bdad62ff234c53aefa5be6c2d87daa25e0f
dangling commit 7b288b661ce6e736be1acfc7ef7a4501bd75d494
dangling commit 62fe1bf8eabe8d0f73f52bcb6b59bcedaf594f59
dangling commit 549b3cb95d459742b36df9f5f6cc40a63da8d260
dangling commit 0dca8c2e4f656e291cbf2a5d2d7a51620a182afb
dangling commit 6402ed233fa7d84f5f9414a67cc6a8547273d2e0
dangling commit ab0a2ddac262490dc31708099efbfada124b966e
dangling commit 751fad2ad795c79ac13af52051055a7960515339
dangling commit 0d303d4d4f3c3dfd2e9f14979bec76732101a9cb
dangling commit 52f7fd5ed7305d8809adeec23eea4e678d9582b9
dangling commit 4d6f5e410d4f40364982c35a6a02e1e574209024
dangling commit 43a9feb7f8705af5008c963e36e36b9b1d69c373
dangling commit 09b0be491bdcf7f76189906674b9d24e90f74787
dangling commit 85b13e0b7fcf4a0536666897bef67e512b5a7db0
dangling commit 10b35e00233e834edc1f373d8dc82ff32b0101a0
dangling commit 2cbfcef666001cd38f6768a115b297a75987a975
dangling commit 22686fdf6a0f02961595ba36c280742b3afbfafa
dangling commit 1a7a1fbd757c1238a0cd0e9c1b43c3828e7c8ae4

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git log  487128950df6ee433c131b5feaafe81ee86629f4
commit 487128950df6ee433c131b5feaafe81ee86629f4
Author: Travis Cross <tc@traviscross.com>
Date:   Fri Mar 21 06:12:02 2014 +0000
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Fri Mar 14 02:59:13 2014 +0500

    Use the system version of APR / APR-util if possible

    Autodetect whether the system libapr / libaprutil has our
    necessary modifications and use it if it does.

commit cde20f6fe68523d9416d2fed72435a8ba880a269
Author: Travis Cross <tc@traviscross.com>
Date:   Thu Mar 20 22:05:23 2014 +0000

    Require sqlite as a system dependency

    This purges sqlite from our tree and requires it to be present on the
    system for building and running FreeSWITCH.

    FS-353

commit 8574988c3a378b4d5861ecaeb0e958657635703b
Author: Travis Cross <tc@traviscross.com>
Date:   Sun Mar 23 21:00:33 2014 +0000
Author: James Le Cuirot <chewi@aura-online.co.uk>
Date:   Sat Mar 22 10:25:54 2014 +0000

    Completely unbundle libedit

    FS-353

    Signed-off-by: Travis Cross <tc@traviscross.com>

commit 1cde5f01e0ffd206c8a455ba89b21e11eabf53a8
Author: Jeff Lenk <jeff@jefflenk.com>
Date:   Sun Mar 23 16:15:49 2014 -0500

    FS-6386 --resolve

commit f4abf3dab4fac67ca0319fb7acdc6c4c5d84cace
Author: Jeff Lenk <jeff@jefflenk.com>
Date:   Sun Mar 23 14:28:42 2014 -0600

    FS-6397 --resolve

commit 7e1b32c0c27567fff17732bb6f1cbfe77d97c6e6
Author: Peter Olsson <peter@olssononline.se>
Date:   Sun Mar 23 10:02:35 2014 +0100

    .gitignore

commit c8fa0f0c4bf0cc8e3d7fec68a9facfc0750f6e54
Author: Peter Olsson <peter@olssononline.se>
Date:   Sun Mar 23 09:25:41 2014 +0100

    mod_v8: Use parallel build by default. Use configure flag "--disable-parallel-build-v8" to disable it. It's disabled by default for Debian build scripts, since parallel build has some issues with cowbuilder.

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase -i 8574988c3a378b4d5861ecaeb0e958657635703b~1
Stopped at 8574988c3a...  Completely unbundle libedit
You can amend the commit now, with

  git commit --amend

Once you are satisfied with your changes, run

  git rebase --continue

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git commit --amend --reset-author
[detached HEAD d8e8b8c2c7] Completely unbundle libedit
 106 files changed, 94 insertions(+), 24047 deletions(-)
 delete mode 100644 cmake_modules/FindLibedit.cmake
 create mode 100644 libs/esl/src/include/esl_config_auto.h.in
 delete mode 100644 libs/libedit/CMakeLists.txt
 delete mode 100644 libs/libedit/COPYING
 delete mode 100644 libs/libedit/ChangeLog
 delete mode 100644 libs/libedit/INSTALL
 delete mode 100644 libs/libedit/Makefile.am
 delete mode 100644 libs/libedit/THANKS
 delete mode 100644 libs/libedit/acinclude.m4
 delete mode 100644 libs/libedit/configure.ac
 delete mode 100644 libs/libedit/configure.gnu
 delete mode 100644 libs/libedit/doc/Makefile.am
 delete mode 100644 libs/libedit/doc/editline.3.roff
 delete mode 100644 libs/libedit/doc/editrc.5.roff
 delete mode 100644 libs/libedit/doc/mdoc2man.awk
 delete mode 100644 libs/libedit/examples/Makefile.am
 delete mode 100644 libs/libedit/examples/fileman.c
 delete mode 100644 libs/libedit/examples/test.c
 delete mode 100644 libs/libedit/libedit.pc.in
 delete mode 100644 libs/libedit/patches/00-vis.h.patch
 delete mode 100644 libs/libedit/patches/01-term.c.patch
 delete mode 100644 libs/libedit/patches/02-el_term.h.patch
 delete mode 100644 libs/libedit/patches/03-unvis.c.patch
 delete mode 100644 libs/libedit/patches/04-strlcpy.c.patch
 delete mode 100644 libs/libedit/patches/05-strlcat.c.patch
 delete mode 100644 libs/libedit/patches/06-readline.c.patch
 delete mode 100644 libs/libedit/patches/07-sys.h.patch
 delete mode 100644 libs/libedit/patches/08-el.h.patch
 delete mode 100644 libs/libedit/patches/09-search.c.patch
 delete mode 100644 libs/libedit/patches/10-readline.h.patch
 delete mode 100644 libs/libedit/patches/11-el.c.patch
 delete mode 100644 libs/libedit/patches/12-history.c.patch
 delete mode 100644 libs/libedit/patches/13-vis.c.patch
 delete mode 100644 libs/libedit/patches/14-fgetln.c.patch
 delete mode 100644 libs/libedit/patches/15-filecomplete.c.patch
 delete mode 100644 libs/libedit/patches/16-tc1.c.patch
 delete mode 100644 libs/libedit/patches/17-editline.3.roff.patch
 delete mode 100644 libs/libedit/patches/18-editrc.5.roff.patch
 delete mode 100644 libs/libedit/patches/README
 delete mode 100755 libs/libedit/patches/cvs_export.sh
 delete mode 100755 libs/libedit/patches/extra_dist_list.sh
 delete mode 100755 libs/libedit/patches/patches_apply.sh
 delete mode 100755 libs/libedit/patches/patches_check.sh
 delete mode 100755 libs/libedit/patches/patches_make.sh
 delete mode 100644 libs/libedit/patches/timestamp.cvsexport
 delete mode 100755 libs/libedit/patches/update_dist.sh
 delete mode 100755 libs/libedit/patches/update_version.sh
 delete mode 100644 libs/libedit/src/CMakeLists.txt
 delete mode 100644 libs/libedit/src/Makefile.am
 delete mode 100644 libs/libedit/src/chared.c
 delete mode 100644 libs/libedit/src/chared.h
 delete mode 100644 libs/libedit/src/common.c
 delete mode 100644 libs/libedit/src/editline/readline.h
 delete mode 100644 libs/libedit/src/el.c
 delete mode 100644 libs/libedit/src/el.h
 delete mode 100644 libs/libedit/src/el_term.h
 delete mode 100644 libs/libedit/src/emacs.c
 delete mode 100644 libs/libedit/src/fgetln.c
 delete mode 100644 libs/libedit/src/filecomplete.c
 delete mode 100644 libs/libedit/src/filecomplete.h
 delete mode 100644 libs/libedit/src/hist.c
 delete mode 100644 libs/libedit/src/hist.h
 delete mode 100644 libs/libedit/src/histedit.h
 delete mode 100644 libs/libedit/src/history.c
 delete mode 100644 libs/libedit/src/key.c
 delete mode 100644 libs/libedit/src/key.h
 delete mode 100644 libs/libedit/src/makelist
 delete mode 100644 libs/libedit/src/map.c
 delete mode 100644 libs/libedit/src/map.h
 delete mode 100644 libs/libedit/src/parse.c
 delete mode 100644 libs/libedit/src/parse.h
 delete mode 100644 libs/libedit/src/prompt.c
 delete mode 100644 libs/libedit/src/prompt.h
 delete mode 100644 libs/libedit/src/read.c
 delete mode 100644 libs/libedit/src/read.h
 delete mode 100644 libs/libedit/src/readline.c
 delete mode 100644 libs/libedit/src/refresh.c
 delete mode 100644 libs/libedit/src/refresh.h
 delete mode 100644 libs/libedit/src/search.c
 delete mode 100644 libs/libedit/src/search.h
 delete mode 100644 libs/libedit/src/shlib_version
 delete mode 100644 libs/libedit/src/sig.c
 delete mode 100644 libs/libedit/src/sig.h
 delete mode 100644 libs/libedit/src/strlcat.c
 delete mode 100644 libs/libedit/src/strlcpy.c
 delete mode 100644 libs/libedit/src/sys.h
 delete mode 100644 libs/libedit/src/term.c
 delete mode 100644 libs/libedit/src/tokenizer.c
 delete mode 100644 libs/libedit/src/tty.c
 delete mode 100644 libs/libedit/src/tty.h
 delete mode 100644 libs/libedit/src/unvis.c
 delete mode 100644 libs/libedit/src/vi.c
 delete mode 100644 libs/libedit/src/vis.c
 delete mode 100644 libs/libedit/src/vis.h

A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase -i 8574988c3a378b4d5861ecaeb0e958657635703b~1
fatal: It seems that there is already a rebase-merge directory, and
I wonder if you are in the middle of another rebase.  If that is the
case, please try
        git rebase (--continue | --abort | --skip)
If that is not the case, please
        rm -fr ".git/rebase-merge"
and run me again.  I am stopping in case you still have something
valuable there.


A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
Stopped at 487128950d...  Use the system version of APR / APR-util if possible
You can amend the commit now, with

  git commit --amend

Once you are satisfied with your changes, run

  git rebase --continue

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git commit --amend --reset-author
[detached HEAD 9f77a2dbda] Use the system version of APR / APR-util if possible
 2 files changed, 37 insertions(+), 3 deletions(-)

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
interactive rebase in progress; onto 1cde5f01e0
Last commands done (939 commands done):
   pick 64060c7dbd Add sofia gateway parameter "destination-prefix"
   pick 1772be2071 FS-5497 add sofia gateway parameter destination-prefix in case you need to send Invites with prefix only to this gateway
Next commands to do (4935 remaining commands):
   pick 6d1469d2fb Describe how to hard-wrap text in Emacs
   pick b2f59dd200 Add warning when using HTTPS with mod_curl
You are currently rebasing branch 'master' on '1cde5f01e0'.

nothing to commit, working tree clean
The previous cherry-pick is now empty, possibly due to conflict resolution.
If you wish to commit it anyway, use:

    git commit --allow-empty

Otherwise, please use 'git reset'
Could not apply 1772be2071... FS-5497 add sofia gateway parameter destination-prefix in case you need to send Invites with prefix only to this gateway

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git commit --allow-empty
[detached HEAD 663080de27] FS-5497 add sofia gateway parameter destination-prefix in case you need to send Invites with prefix only to this gateway
 Author: stangor <stangor1@gmail.com>
 Date: Thu Aug 14 17:28:14 2014 -0700

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
warning: inexact rename detection was skipped due to too many files.
warning: you may want to set your merge.renamelimit variable to at least 3185 and retry the command.
warning: inexact rename detection was skipped due to too many files.
warning: you may want to set your merge.renamelimit variable to at least 3185 and retry the command.
warning: inexact rename detection was skipped due to too many files.
warning: you may want to set your merge.renamelimit variable to at least 3185 and retry the command.
Auto-merging libs/freetdm/src/include/private/ftdm_core.h
CONFLICT (content): Merge conflict in libs/freetdm/src/include/private/ftdm_core.h
Auto-merging libs/freetdm/src/ftdm_io.c
Auto-merging libs/freetdm/mod_freetdm/mod_freetdm.c
warning: inexact rename detection was skipped due to too many files.
warning: you may want to set your merge.renamelimit variable to at least 3185 and retry the command.
error: could not apply b80cdd45d5... freetdm: Added release guard time configuration
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply b80cdd45d5... freetdm: Added release guard time configuration

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git log
commit cf983eea149307ccd178fd1b3f412550749b74fc (HEAD)
Author: Moises Silva <moy@sangoma.com>
Date:   Tue Jul 22 23:44:17 2014 -0400

    freetdm: Raise some buffer limits

commit c2b8f2a525094e94abc68d81900b16856a67ea0b
Author: Moises Silva <moy@sangoma.com>
Date:   Sun Jul 20 21:51:32 2014 -0400

    freetdm: ftmod_analog_em: Added support for suspending channels that are offhook

commit bf24d113c69faee8693feb4fa8403be5d037b6f6
Author: Moises Silva <moy@sangoma.com>
Date:   Sun Jul 20 21:45:24 2014 -0400

    mod_freetdm: Added 'ftdm cas' command to read/write raw CAS bits

commit 899678bcaf7796ed6fee84dfaf1df3f84e9d57ca
Author: Moises Silva <moy@sangoma.com>
Date:   Sun Nov 9 03:33:08 2014 -0500

    OPENZAP-232 #resolve
    Patched-By: Florian Richter

    Check for digits received on sangoma isdn stack to avoid delaying
    moving to the ring state if all digits are received at once in
    overlap dialing mode

commit 1d92c338e1714fb59484d46cc22f8d592b8a40f4
Author: Ken Rice <krice@freeswitch.org>
Date:   Sat Nov 8 15:13:05 2014 -0600

    Revert "FS-6967 New mod_say_es_AR to support Argentina Spanish variant."
    This reverts commit e75d0675afd8974687931143709814544299fadc.

    Revert "Add mod_say_es_ar to debian packaging"
    This reverts commit ebb3c8fbfa3ec8b9f4b4a2ef20d62d343c8ff6c4.

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> :q
A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git log
commit cf983eea149307ccd178fd1b3f412550749b74fc (HEAD)
Author: Moises Silva <moy@sangoma.com>
Date:   Tue Jul 22 23:44:17 2014 -0400

    freetdm: Raise some buffer limits

commit c2b8f2a525094e94abc68d81900b16856a67ea0b
Author: Moises Silva <moy@sangoma.com>
Date:   Sun Jul 20 21:51:32 2014 -0400

    freetdm: ftmod_analog_em: Added support for suspending channels that are offhook

commit bf24d113c69faee8693feb4fa8403be5d037b6f6
Author: Moises Silva <moy@sangoma.com>
Date:   Sun Jul 20 21:45:24 2014 -0400

    mod_freetdm: Added 'ftdm cas' command to read/write raw CAS bits

commit 899678bcaf7796ed6fee84dfaf1df3f84e9d57ca
Author: Moises Silva <moy@sangoma.com>
Date:   Sun Nov 9 03:33:08 2014 -0500

    OPENZAP-232 #resolve
    Patched-By: Florian Richter

    Check for digits received on sangoma isdn stack to avoid delaying
    moving to the ring state if all digits are received at once in
    overlap dialing mode

commit 1d92c338e1714fb59484d46cc22f8d592b8a40f4
Author: Ken Rice <krice@freeswitch.org>
Date:   Sat Nov 8 15:13:05 2014 -0600

    Revert "FS-6967 New mod_say_es_AR to support Argentina Spanish variant."
    This reverts commit e75d0675afd8974687931143709814544299fadc.

    Revert "Add mod_say_es_ar to debian packaging"
    This reverts commit ebb3c8fbfa3ec8b9f4b4a2ef20d62d343c8ff6c4.

    Conflicts:
            configure.ac

commit b2933ee987be60c1829eeeec222c5fad6da46f08
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Fri Nov 7 17:11:40 2014 -0600

    fix regression caused by missing ! char in commit: 4eb5b388

commit c01d2eda8a1f47dd9e08801dc85ed4db3ef04521
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Fri Nov 7 17:10:53 2014 -0600

    add command to comppile non-minified js file for testing

commit 291303b4e0de2a6331600302408876a2a0c75132
Author: Brian West <brian@freeswitch.org>

A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
interactive rebase in progress; onto 1cde5f01e0
Last commands done (1329 commands done):
   pick 09198ee357 freetdm: Raise some buffer limits
   pick b80cdd45d5 freetdm: Added release guard time configuration
  (see more in file .git/rebase-merge/done)
Next commands to do (4545 remaining commands):
   pick d65716d83a freetdm: Added dtmf_time_on and dtmf_time_off parameters to tweak DTMF duration in milliseconds
   pick 6b8d5b2b10 freetdm: Fix release guard timer check
  (use "git rebase --edit-todo" to view and edit)
You are currently rebasing branch 'master' on '1cde5f01e0'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   libs/freetdm/mod_freetdm/mod_freetdm.c
        modified:   libs/freetdm/src/ftdm_io.c
        modified:   libs/freetdm/src/ftmod/ftmod_analog_em/ftmod_analog_em.c

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

        both modified:   libs/freetdm/src/include/private/ftdm_core.h


A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git add  libs/freetdm/src/include/private/ftdm_core.h

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
interactive rebase in progress; onto 1cde5f01e0
Last commands done (1329 commands done):
   pick 09198ee357 freetdm: Raise some buffer limits
   pick b80cdd45d5 freetdm: Added release guard time configuration
  (see more in file .git/rebase-merge/done)
Next commands to do (4545 remaining commands):
   pick d65716d83a freetdm: Added dtmf_time_on and dtmf_time_off parameters to tweak DTMF duration in milliseconds
   pick 6b8d5b2b10 freetdm: Fix release guard timer check
  (use "git rebase --edit-todo" to view and edit)
You are currently rebasing branch 'master' on '1cde5f01e0'.
  (all conflicts fixed: run "git rebase --continue")

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   libs/freetdm/mod_freetdm/mod_freetdm.c
        modified:   libs/freetdm/src/ftdm_io.c
        modified:   libs/freetdm/src/ftmod/ftmod_analog_em/ftmod_analog_em.c
        modified:   libs/freetdm/src/include/private/ftdm_core.h


A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
[detached HEAD a93d8012f9] freetdm: Added release guard time configuration
 Author: Moises Silva <moy@sangoma.com>
 4 files changed, 34 insertions(+), 4 deletions(-)
warning: inexact rename detection was skipped due to too many files.
warning: you may want to set your merge.renamelimit variable to at least 3185 and retry the command.
warning: inexact rename detection was skipped due to too many files.
warning: you may want to set your merge.renamelimit variable to at least 3185 and retry the command.
error: unable to create file html5/verto/video_demo/js/verto-min.js: Permission denied
hint: Could not execute the todo command
hint:
hint:     pick ae05a19edfa0c36b820a8285c1b42c2f95849f1e FS-8401 refactor the sinkid function into verto lib
hint:
hint: It has been rescheduled; To edit the command before continuing, please
hint: edit the todo list first:
hint:
hint:     git rebase --edit-todo
hint:     git rebase --continue
Could not apply ae05a19edf... FS-8401 refactor the sinkid function into verto lib

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
interactive rebase in progress; onto 1cde5f01e0
Last commands done (3423 commands done):
   pick ae05a19edf FS-8401 refactor the sinkid function into verto lib
   pick fb8d4a2b99 FS-8545 do not allow video floor on a member with a reservation id set
  (see more in file .git/rebase-merge/done)
Next commands to do (2453 remaining commands):
   pick ae05a19edf FS-8401 refactor the sinkid function into verto lib
   pick 9ab210e64b FS-8556 #resolve [Screen shares are not recoverable so do not try]
  (use "git rebase --edit-todo" to view and edit)
You are currently editing a commit while rebasing branch 'master' on '1cde5f01e0'.
  (use "git commit --amend" to amend the current commit)
  (use "git rebase --continue" once you are satisfied with your changes)

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   html5/verto/js/src/jquery.verto.js
        modified:   html5/verto/verto_communicator/src/index.html
        modified:   html5/verto/verto_communicator/src/vertoControllers/controllers/MainController.js
        deleted:    html5/verto/video_demo/js/verto-min.js
        modified:   html5/verto/video_demo/verto.js

no changes added to commit (use "git add" and/or "git commit -a")

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
interactive rebase in progress; onto 1cde5f01e0
Last commands done (3423 commands done):
   pick ae05a19edf FS-8401 refactor the sinkid function into verto lib
   pick fb8d4a2b99 FS-8545 do not allow video floor on a member with a reservation id set
  (see more in file .git/rebase-merge/done)
Next commands to do (2453 remaining commands):
   pick ae05a19edf FS-8401 refactor the sinkid function into verto lib
   pick 9ab210e64b FS-8556 #resolve [Screen shares are not recoverable so do not try]
  (use "git rebase --edit-todo" to view and edit)
You are currently editing a commit while rebasing branch 'master' on '1cde5f01e0'.
  (use "git commit --amend" to amend the current commit)
  (use "git rebase --continue" once you are satisfied with your changes)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   html5/verto/js/src/jquery.verto.js
        modified:   html5/verto/verto_communicator/src/index.html
        modified:   html5/verto/verto_communicator/src/vertoControllers/controllers/MainController.js
        modified:   html5/verto/video_demo/js/verto-min.js
        modified:   html5/verto/video_demo/verto.js

no changes added to commit (use "git add" and/or "git commit -a")

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
You must edit all merge conflicts and then
mark them as resolved using git add

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git add html5/verto/js/src/jquery.verto.js html5/verto/verto_communicator/src/index.html  html5/verto/verto_communicator/src/vertoControllers/controllers/MainController.js html5/verto/video_demo/js/verto-min.js  html5/verto/video_demo/verto.js

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
interactive rebase in progress; onto 1cde5f01e0
Last commands done (3423 commands done):
   pick ae05a19edf FS-8401 refactor the sinkid function into verto lib
   pick fb8d4a2b99 FS-8545 do not allow video floor on a member with a reservation id set
  (see more in file .git/rebase-merge/done)
Next commands to do (2453 remaining commands):
   pick ae05a19edf FS-8401 refactor the sinkid function into verto lib
   pick 9ab210e64b FS-8556 #resolve [Screen shares are not recoverable so do not try]
  (use "git rebase --edit-todo" to view and edit)
You are currently editing a commit while rebasing branch 'master' on '1cde5f01e0'.
  (use "git commit --amend" to amend the current commit)
  (use "git rebase --continue" once you are satisfied with your changes)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   html5/verto/js/src/jquery.verto.js
        modified:   html5/verto/verto_communicator/src/index.html
        modified:   html5/verto/verto_communicator/src/vertoControllers/controllers/MainController.js
        modified:   html5/verto/video_demo/js/verto-min.js
        modified:   html5/verto/video_demo/verto.js


A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
Auto packing the repository in background for optimum performance.
See "git help gc" for manual housekeeping.
Enumerating objects: 41634, done.
Counting objects: 100% (11039/11039), done.
Delta compression using up to 12 threads
Compressing objects: 100% (11034/11034), done.
Writing objects: 100% (11039/11039), done.
Total 11039 (delta 7516), reused 0 (delta 0)
Removing duplicate objects: 100% (256/256), done.
[detached HEAD f6db7c78f3] FS-8401 refactor the sinkid function into verto lib
 Author: Anthony Minessale <anthm@freeswitch.org>
 5 files changed, 53 insertions(+), 33 deletions(-)
interactive rebase in progress; onto 1cde5f01e0
Last commands done (3510 commands done):
   pick 9f52ebf257 FS-8658: [mod_verto] windows fixes for mod_verto
   pick 8670e5f801 FS-8642 addtl patch
Next commands to do (2366 remaining commands):
   pick 881197e661 FS-8629 #resolve [Add new param video-mute-exit-canvas to conference cflags]
   pick 3fd416166a FS-8663 #resolve [add vid-personal command]
You are currently rebasing branch 'master' on '1cde5f01e0'.

nothing to commit, working tree clean
The previous cherry-pick is now empty, possibly due to conflict resolution.
If you wish to commit it anyway, use:

    git commit --allow-empty

Otherwise, please use 'git reset'
Could not apply 8670e5f801... FS-8642 addtl patch

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
interactive rebase in progress; onto 1cde5f01e0
Last commands done (3510 commands done):
   pick 9f52ebf257 FS-8658: [mod_verto] windows fixes for mod_verto
   pick 8670e5f801 FS-8642 addtl patch
  (see more in file .git/rebase-merge/done)
Next commands to do (2366 remaining commands):
   pick 881197e661 FS-8629 #resolve [Add new param video-mute-exit-canvas to conference cflags]
   pick 3fd416166a FS-8663 #resolve [add vid-personal command]
  (use "git rebase --edit-todo" to view and edit)
You are currently rebasing branch 'master' on '1cde5f01e0'.
  (all conflicts fixed: run "git rebase --continue")

nothing to commit, working tree clean

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git commit --allow-empty
[detached HEAD 484993259f] FS-8642 addtl patch
 Author: Anthony Minessale <anthm@freeswitch.org>
 Date: Mon Dec 14 16:05:51 2015 -0600

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
interactive rebase in progress; onto 1cde5f01e0
Last commands done (4242 commands done):
   pick ba99263a74 column update with negative nibble rate -- FS-9577
   pick 9ad6a159cf column update with negative nibble rate
Next commands to do (1634 remaining commands):
   pick c409499cd9 FS-9576 #resolve [Add Realtime Text]
   pick 7dd872e9b8 FS-9575 #resolve [Add MRCP]
You are currently rebasing branch 'master' on '1cde5f01e0'.

nothing to commit, working tree clean
The previous cherry-pick is now empty, possibly due to conflict resolution.
If you wish to commit it anyway, use:

    git commit --allow-empty

Otherwise, please use 'git reset'
Could not apply 9ad6a159cf... column update with negative nibble rate

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git commit --allow-empty
[detached HEAD 22ee684ee6] column update with negative nibble rate
 Author: Michael Mavroudis <michael@mavroudis.com>
 Date: Tue Sep 27 14:26:12 2016 -0700

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
Auto-merging src/switch_rtp.c
Auto-merging src/switch_ivr_bridge.c
Auto-merging src/switch_core_media.c
CONFLICT (content): Merge conflict in src/switch_core_media.c
Auto-merging src/mod/endpoints/mod_verto/mod_verto.c
Auto-merging src/include/switch_types.h
Auto-merging src/include/switch_core_media.h
error: could not apply c409499cd9... FS-9576 #resolve [Add Realtime Text]
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply c409499cd9... FS-9576 #resolve [Add Realtime Text]

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
interactive rebase in progress; onto 1cde5f01e0
Last commands done (4243 commands done):
   pick 9ad6a159cf column update with negative nibble rate
   pick c409499cd9 FS-9576 #resolve [Add Realtime Text]
  (see more in file .git/rebase-merge/done)
Next commands to do (1633 remaining commands):
   pick 7dd872e9b8 FS-9575 #resolve [Add MRCP]
   pick 6d6bd1efa5 FS-9242 convert to adapter.js
  (use "git rebase --edit-todo" to view and edit)
You are currently rebasing branch 'master' on '1cde5f01e0'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   configure.ac
        modified:   html5/verto/js/src/jquery.verto.js
        modified:   html5/verto/video_demo/index.html
        modified:   html5/verto/video_demo/js/verto-min.js
        modified:   html5/verto/video_demo/verto.js
        modified:   libs/esl/src/esl_event.c
        modified:   libs/esl/src/include/esl_event.h
        modified:   libs/sofia-sip/libsofia-sip-ua/sdp/sdp.bnf
        modified:   libs/sofia-sip/libsofia-sip-ua/sdp/sdp_parse.c
        modified:   libs/sofia-sip/libsofia-sip-ua/sdp/sdp_print.c
        modified:   libs/sofia-sip/libsofia-sip-ua/sdp/sofia-sip/sdp.h
        modified:   src/include/private/switch_core_pvt.h
        modified:   src/include/switch_buffer.h
        modified:   src/include/switch_channel.h
        modified:   src/include/switch_core.h
        modified:   src/include/switch_core_event_hook.h
        modified:   src/include/switch_core_media.h
        modified:   src/include/switch_ivr.h
        modified:   src/include/switch_jitterbuffer.h
        modified:   src/include/switch_module_interfaces.h
        modified:   src/include/switch_types.h
        modified:   src/include/switch_utils.h
        modified:   src/mod/applications/mod_av/avformat.c
        modified:   src/mod/applications/mod_commands/mod_commands.c
        modified:   src/mod/applications/mod_conference/conference_member.c
        modified:   src/mod/applications/mod_conference/mod_conference.c
        modified:   src/mod/applications/mod_conference/mod_conference.h
        modified:   src/mod/applications/mod_dptools/mod_dptools.c
        modified:   src/mod/applications/mod_fsv/mod_fsv.c
        modified:   src/mod/endpoints/mod_rtc/mod_rtc.c
        modified:   src/mod/endpoints/mod_sofia/mod_sofia.c
        modified:   src/mod/endpoints/mod_sofia/rtp.c
        modified:   src/mod/endpoints/mod_sofia/sofia_glue.c
        modified:   src/mod/endpoints/mod_verto/mcast/mcast.c
        modified:   src/mod/endpoints/mod_verto/mod_verto.c
        modified:   src/mod/endpoints/mod_verto/mod_verto.h
        modified:   src/switch_buffer.c
        modified:   src/switch_core_media_bug.c
        modified:   src/switch_core_session.c
        modified:   src/switch_event.c
        modified:   src/switch_ivr.c
        modified:   src/switch_ivr_async.c
        modified:   src/switch_ivr_bridge.c
        modified:   src/switch_jitterbuffer.c
        modified:   src/switch_rtp.c

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

        both modified:   src/switch_core_media.c


A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git add src/switch_core_media.c

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
[detached HEAD cc5fa81941] FS-9576 #resolve [Add Realtime Text]
 Author: Anthony Minessale <anthm@freeswitch.org>
 46 files changed, 2934 insertions(+), 402 deletions(-)
warning: Cannot merge binary files: html5/verto/video_demo/js/verto-min.js (HEAD vs. 6d6bd1efa5... FS-9242 convert to adapter.js)
Auto-merging src/switch_rtp.c
Auto-merging src/switch_core_media.c
Auto-merging html5/verto/video_demo/verto.js
Auto-merging html5/verto/video_demo/js/verto-min.js
CONFLICT (content): Merge conflict in html5/verto/video_demo/js/verto-min.js
error: could not apply 6d6bd1efa5... FS-9242 convert to adapter.js
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 6d6bd1efa5... FS-9242 convert to adapter.js

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
interactive rebase in progress; onto 1cde5f01e0
Last commands done (4245 commands done):
   pick 7dd872e9b8 FS-9575 #resolve [Add MRCP]
   pick 6d6bd1efa5 FS-9242 convert to adapter.js
  (see more in file .git/rebase-merge/done)
Next commands to do (1631 remaining commands):
   pick 2511ad50e1 FS-8955 [verto_communicator] Adding DTMF shortcuts and handling DTMF history on DTMF widget
   pick fa7cb3d546 FS-9508 [verto_communicator] Adding AGC option on settings, enabled by default
  (use "git rebase --edit-todo" to view and edit)
You are currently rebasing branch 'master' on '1cde5f01e0'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   html5/verto/js/Makefile
        modified:   html5/verto/js/src/jquery.FSRTC.js
        new file:   html5/verto/js/src/vendor/adapter-latest.js
        modified:   html5/verto/verto_communicator/src/index.html
        modified:   html5/verto/verto_communicator/src/partials/preview.html
        modified:   html5/verto/verto_communicator/src/storageService/services/splash_screen.js
        modified:   html5/verto/verto_communicator/src/storageService/services/storage.js
        modified:   html5/verto/verto_communicator/src/vertoControllers/controllers/DialPadController.js
        modified:   html5/verto/verto_communicator/src/vertoControllers/controllers/PreviewController.js
        modified:   html5/verto/verto_communicator/src/vertoControllers/controllers/SettingsController.js
        modified:   html5/verto/verto_communicator/src/vertoControllers/controllers/SplashScreenController.js
        modified:   html5/verto/verto_communicator/src/vertoDirectives/directives/videoTag.js
        modified:   html5/verto/verto_communicator/src/vertoService/services/vertoService.js
        modified:   html5/verto/video_demo/verto.js
        modified:   src/switch_core_media.c
        modified:   src/switch_rtp.c

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

        both modified:   html5/verto/video_demo/js/verto-min.js


A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git add html5/verto/video_demo/js/verto-min.js

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
[detached HEAD 0607d03bc7] FS-9242 convert to adapter.js
 Author: Anthony Minessale <anthm@freeswitch.org>
 17 files changed, 4132 insertions(+), 288 deletions(-)
 create mode 100644 html5/verto/js/src/vendor/adapter-latest.js
interactive rebase in progress; onto 1cde5f01e0
Last commands done (4271 commands done):
   pick d429cc2f5a FS-9552
   pick de223ea2c6 FS-9593 #resolve [Video syncs too much on video muted channels] %backport=1.6
Next commands to do (1605 remaining commands):
   pick 707502ff24 FS-9597 #resolve [host_lookup does not resolve v6 addrs] %backport=1.6
   pick c6ece47314 FS-9596 #resolve [rtp-timeout triggered for on-hold calls with a=inactive]
You are currently rebasing branch 'master' on '1cde5f01e0'.

nothing to commit, working tree clean
The previous cherry-pick is now empty, possibly due to conflict resolution.
If you wish to commit it anyway, use:

    git commit --allow-empty

Otherwise, please use 'git reset'
Could not apply de223ea2c6... FS-9593 #resolve [Video syncs too much on video muted channels] %backport=1.6

A@DESKTOP-84IQ1ND D:\work\freeswitch2
>  git commit --allow-empty
[detached HEAD 1d810c132c] FS-9593 #resolve [Video syncs too much on video muted channels] %backport=1.6
 Author: Anthony Minessale <anthm@freeswitch.org>
 Date: Thu Sep 29 17:13:14 2016 -0500

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
interactive rebase in progress; onto 1cde5f01e0
Last commands done (4275 commands done):
   pick b24c2ac945 FS-9595 [avmd] Extend avmd show api
   pick c7e983d896 FS-9597 #resolve [host_lookup does not resolve v6 addrs] %backport=1.6
Next commands to do (1601 remaining commands):
   pick 17ab107ed7 FS-9596 #resolve [rtp-timeout triggered for on-hold calls with a=inactive]
   pick 184368d8f4 FS-9568 [avmd] Fail session creation if can't be started properly
You are currently rebasing branch 'master' on '1cde5f01e0'.

nothing to commit, working tree clean
The previous cherry-pick is now empty, possibly due to conflict resolution.
If you wish to commit it anyway, use:

    git commit --allow-empty

Otherwise, please use 'git reset'
Could not apply c7e983d896... FS-9597 #resolve [host_lookup does not resolve v6 addrs] %backport=1.6

A@DESKTOP-84IQ1ND D:\work\freeswitch2
>  git commit --allow-empty
[detached HEAD 45174b10bd] FS-9597 #resolve [host_lookup does not resolve v6 addrs] %backport=1.6
 Author: Anthony Minessale <anthm@freeswitch.org>
 Date: Fri Sep 30 12:12:04 2016 -0500

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
interactive rebase in progress; onto 1cde5f01e0
Last commands done (4276 commands done):
   pick c7e983d896 FS-9597 #resolve [host_lookup does not resolve v6 addrs] %backport=1.6
   pick 17ab107ed7 FS-9596 #resolve [rtp-timeout triggered for on-hold calls with a=inactive]
Next commands to do (1600 remaining commands):
   pick 184368d8f4 FS-9568 [avmd] Fail session creation if can't be started properly
   pick f47bbecec9 FS-9601: mod_opus:  make adjustable bitrate mutually exclusive with FEC enforcing on the decreasing trend, add step calculation for bitrate adjustment, fix bug on context settings
You are currently rebasing branch 'master' on '1cde5f01e0'.

nothing to commit, working tree clean
The previous cherry-pick is now empty, possibly due to conflict resolution.
If you wish to commit it anyway, use:

    git commit --allow-empty

Otherwise, please use 'git reset'
Could not apply 17ab107ed7... FS-9596 #resolve [rtp-timeout triggered for on-hold calls with a=inactive]

A@DESKTOP-84IQ1ND D:\work\freeswitch2
>  git commit --allow-empty
[detached HEAD 43eb7bd683] FS-9596 #resolve [rtp-timeout triggered for on-hold calls with a=inactive]
 Author: Anthony Minessale <anthm@freeswitch.org>
 Date: Fri Sep 30 12:57:54 2016 -0500

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
Auto-merging src/switch_core_media.c
CONFLICT (content): Merge conflict in src/switch_core_media.c
error: could not apply 2a3b8a230c... FS-9610 #resolve [Video keyframe requests not being propagated properly] %backport=1.6
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 2a3b8a230c... FS-9610 #resolve [Video keyframe requests not being propagated properly] %backport=1.6

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
interactive rebase in progress; onto 1cde5f01e0
Last commands done (4284 commands done):
   pick 15a232b5bb FS-9609 - [mod_callcenter] BLF for Queues, callcenter_track app, EXIT_WITH_KEY reason and xml_curl improvements
   pick 2a3b8a230c FS-9610 #resolve [Video keyframe requests not being propagated properly] %backport=1.6
  (see more in file .git/rebase-merge/done)
Next commands to do (1592 remaining commands):
   pick 9a990add75 FS-9612 #resolve [RTCP-MUX wrongly enabled in cases where answer contains rtcp but offer didn't / remote addr not obtained in UDPTL mode] %backport=1.6
   pick 4583ac8c98 FS-9605: [avmd] Add number of detection threads setting to config
  (use "git rebase --edit-todo" to view and edit)
You are currently rebasing branch 'master' on '1cde5f01e0'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   src/switch_ivr_bridge.c
        modified:   src/switch_rtp.c

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

        both modified:   src/switch_core_media.c


A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git add src/switch_core_media.c

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
[detached HEAD 351a32ca4b] FS-9610 #resolve [Video keyframe requests not being propagated properly] %backport=1.6
 Author: Anthony Minessale <anthm@freeswitch.org>
 3 files changed, 222 insertions(+), 106 deletions(-)
warning: Cannot merge binary files: html5/verto/video_demo/js/verto-min.js (HEAD vs. 5d6ab013d1... FS-9628: [verto] correct return value of $.FSRTC.bestResSupported to be the largest resolution)
Auto-merging html5/verto/video_demo/js/verto-min.js
CONFLICT (content): Merge conflict in html5/verto/video_demo/js/verto-min.js
error: could not apply 5d6ab013d1... FS-9628: [verto] correct return value of $.FSRTC.bestResSupported to be the largest resolution
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 5d6ab013d1... FS-9628: [verto] correct return value of $.FSRTC.bestResSupported to be the largest resolution

A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
interactive rebase in progress; onto 1cde5f01e0
Last commands done (4310 commands done):
   pick 7d7200f03c FS-9624 #resolve [Jitter buffer doesn't flush properly on RTT sessions]
   pick 5d6ab013d1 FS-9628: [verto] correct return value of $.FSRTC.bestResSupported to be the largest resolution
  (see more in file .git/rebase-merge/done)
Next commands to do (1566 remaining commands):
   pick b10aabb94f FS-9623 update .update
   pick 11f4b3c4f7 FS-9575 fix windows build
  (use "git rebase --edit-todo" to view and edit)
You are currently rebasing branch 'master' on '1cde5f01e0'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   html5/verto/js/src/jquery.FSRTC.js

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

        both modified:   html5/verto/video_demo/js/verto-min.js


A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git add html5/verto/video_demo/js/verto-min.js

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
[detached HEAD ca16789564] FS-9628: [verto] correct return value of $.FSRTC.bestResSupported to be the largest resolution
 Author: Mike Jerris <mike@jerris.com>
 2 files changed, 1 insertion(+), 1 deletion(-)
Successfully rebased and updated refs/heads/master.
```

the v1.8_new branch fork log:
```

>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase 16d600c0350a79c2532c739dd1432f7ed318ea09^ 4d4c454d3ef694767fbcefb3e444d23301dbbcab --onto v1.8_new
fatal: Does not point to a valid commit 'v1.8_new'

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase 16d600c0350a79c2532c739dd1432f7ed318ea09^ 4d4c454d3ef694767fbcefb3e444d23301dbbcab --onto v1.8_new
First, rewinding head to replay your work on top of it...
Generating patches: 100% (2830/2830), done.
Applying: FS-7486: Fix handling of queued requests in Sofia-SIP
Using index info to reconstruct a base tree...
M       libs/sofia-sip/libsofia-sip-ua/nua/nua_client.c
Falling back to patching base and 3-way merge...
Auto-merging libs/sofia-sip/libsofia-sip-ua/nua/nua_client.c
No changes -- Patch already applied.
Applying: Making mod_rtmp compatible with Adobe Media Server
.git/rebase-apply/patch:93: trailing whitespace.

warning: 1 line adds whitespace errors.
Using index info to reconstruct a base tree...
M       src/mod/endpoints/mod_rtmp/mod_rtmp.c
M       src/mod/endpoints/mod_rtmp/mod_rtmp.h
M       src/mod/endpoints/mod_rtmp/rtmp_sig.c
Falling back to patching base and 3-way merge...
Auto-merging src/mod/endpoints/mod_rtmp/rtmp_sig.c
CONFLICT (content): Merge conflict in src/mod/endpoints/mod_rtmp/rtmp_sig.c
Auto-merging src/mod/endpoints/mod_rtmp/mod_rtmp.h
Auto-merging src/mod/endpoints/mod_rtmp/mod_rtmp.c
error: Failed to merge in the changes.
hint: Use 'git am --show-current-patch' to see the failed patch
Patch failed at 0002 Making mod_rtmp compatible with Adobe Media Server
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
rebase in progress; onto b0c3918cbf
You are currently rebasing.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

        both modified:   src/mod/endpoints/mod_rtmp/rtmp_sig.c

no changes added to commit (use "git add" and/or "git commit -a")

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --abort

A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
HEAD detached at refs/heads/v1.8
nothing to commit, working tree clean

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase f7e2505fc7b007f29b4c61de1cf2337b07e3b589^ 4d4c454d3ef694767fbcefb3e444d23301dbbcab --onto v1.8_new
First, rewinding head to replay your work on top of it...
Generating patches: 100% (444/444), done.
Applying: FS-9096: [mod_commands] new uuid argument for bgapi
Applying: FS-10814 - [verto_communicator] Do not change video resolution if auto detect video settings is off
Applying: FS-10815 #resove
Applying: FS-10805: Memory Leak fix in mod_amqp
Applying: FS-10789: [mod_v8] v8 segs on invalid instruction
Applying: FS-10496: [mod_v8] Speedup JavaScript. Enabling Code Caching.
Applying: FS-10826 [core] Fix file handler sample rate after transfer
Applying: FS-10828 [mod_verto] Always renew codec_string on connect
Applying: FS-10832 #resolve
Applying: FS-10833: [mod_amqp] memory leak on reconnection attempt to RabbitMQ broker
Applying: FS-10858 - [verto_communicator] Waiting for resolution check finish before redirecting to preview screen.
Applying: FS-10859 #resolve
Applying: FS-10858 - [verto_communicator] Removing emit of res_check_done, on slow connections the emit could happen before the listening thus freezing the app on loading
Applying: FS-10858 - [verto_communicator] cancel timeout when done to avoid redirect loop
Applying: A tweak to the PCAP file parsing code in spandsp to allow for 802.1Q headers in
Applying: FS-10875: #resolve Portability issues for non-bash interpreters.
Applying: FS-10881: Debian sources parsing support for args
Applying: FS-10876: [Build-System] Fix build in Visual Studio 2017 and Windows SDK 10.
Applying: FS-10820 [mod_kazoo] eventstream configuration
Applying: FS-10611: [mod_commands] Add domain_data command to retrieve domain information
Applying: Tweaks and feature additions to some of the spandsp tests.
Applying: FS-10299: [mod_callcenter] Add an option to disable global database lock on mod_callcenter
Applying: FS-10521 FS-10612 [mod_callcenter] Member exit reason set to EXIT_WITH_KEY when it should be TIMEOUT and only set EXIT_WITH_KEY if the key pressed is set at cc_exit_keys
Applying: [fix](verto_communicator) add grunt-cli dependency to packages.json
Applying: Revert "FS-10820 [mod_kazoo] eventstream configuration"
Applying: FS-10496: [mod_v8] Fixing regression from FS-10496 when no settings exist in v8.conf.
Applying: FS-6816 [mod_sofia] Set empty callee id if `_undef_`
Applying: FS-10939 mod_cdr_mongodb: fix format truncation warnings with gcc 7
Applying: FS-10493: [mod_callcenter] Replace core uuid with cc_instance_id taken from callcenter.conf.xml
Applying: FS-9753: [mod_sofia] Fix crash when accessing the WSS interface via regular HTTPS
Applying: fix FS-9298
Applying: FS-10777 [mod_erlang_event] #resolve
Applying: Improve recording transfer
Applying: Skip holding b leg only if it is on hold
Applying: FS-11061: [core] fix build with newer pcre
Applying: FS-11046: [mod_dptools] Better logging for rename_function
Applying: FS-8893: [mod_sofia] Add variables to sofia::register/unregister events
Applying: FS-10119 [mod_h323] Patch from issue
Applying: FS-11088: Fixing verto status api
Applying: FS-11099 [mod_conference] provide profile name when requesting for
Applying: FS-11100 [mod_conference] export variables for conference_outcall_bg
Applying: FS-11104: [Build] check for libldns-fs.pc first when detecting libldns to allow for custom package
Applying: FS-11108: [mod_commands] Fix segfault with list_users command
Applying: FS-10775 #resolve segfault switch_frame_buffer_push
Applying: FS-11120 Handle invalid configs for avmd
Applying: FS-11044 - [verto_communicator] Fix error when muting/unmuting without cam and/or mic.
Applying: FS-11139: [mod_commands] add tab completion for uuid_pause command
Applying: FS-10808 [mod_sofia] Fix memory leak in SLA
Applying: FS-11168: [core] fix compile error on gentoo from typo in assert statement
Applying: FS-11176: [core] do not use previous codec if its not ready
Applying: FI-393 [fs_cli banner] this commit changes the text on the fs_cli banner
Applying: FS-11198: [Debian] install fonts to the correct location
Applying: FS-11211 - [verto_communicator]: Adding turnServer and socketFallbackUrl options.
Applying: FS-11227 - [verto_communicator]: stops localVideoStream tracks properly
Applying: FS-11229 - [verto_communicator]: deprecating use of URL.createObjectURL
Applying: FS-11230: [core] Fix bad rtp timestamps triggered by cng/missed packet detection
Applying: FS-11230: [core] Fix bad rtp timestamps triggered by cng/missed packet detection
Applying: FS-11236 [verto_communicator] call useCamera property on video constraints
Applying: FS-11262 [verto_communicator] prevent vertojs from stop retrying socket connection by undefined socketfallbackurl
Applying: FS-10784: [freeswitch-core] Make Users lists compatible with all forms of xml #resolve
Applying: FS-10762: [freeswitch-core] Websocket logic error
Applying: FS-10762: [freeswitch-core] Websocket logic error #resolve
Applying: FS-8761: [freeswitch-core] Memory leak in FreeSWITCH
Applying: FS-10778: Add support for MKI to SRTP
Applying: FS-10778: fix MKI compile error
Applying: FS-10778: Fix compilation and refactor code
Applying: FS-10823: [mod_sofia] curly brackets on SDP header causes FS to crash #resolve
Applying: FS-10778: Evaluate rtp_secure_media_mki variable with switch_channel_var_true
Applying: FS-10843: [freeswitch-core] Tweak RTP write timing #resolve
Applying: FS-10853: Fix failed build for mod_dingaling
Applying: swigall
Applying: FS-10778: Fix for MKI regression introduced in FS-10778
Applying: FS-10762: [freeswitch-core] Websocket logic error #resolve
Applying: FS-10770: [freeswitch-core] Make nack buffer bigger by default
Applying: FS-10770: [freeswitch-core] Make nack buffer bigger by default
Applying: FS-10762: [freeswitch-core] Websocket logic error
Applying: FS-10762: [freeswitch-core] Websocket logic error #resolve
Applying: FS-10799: [mod_commands] Add toupper and tolower api funcs #resolve
Applying: FS-8761: [freeswitch-core] Memory leak in FreeSWITCH
Applying: remove hack for chrome we don't need anymore
Applying: FS-10802: [mod_conference] Convert conference floor to id based #resolve
Applying: FS-10803: [mod_conference] Add support for alternate video layout config per conference profile #resolve
Applying: FS-10803: [mod_conference] Add support for alternate video layout config per conference profile
Applying: FS-10802: [mod_conference] Convert conference floor to id based
Applying: FS-10802: [mod_conference] Convert conference floor to id based
Applying: FS-10769: [mod_av,mod_conference] Lipsync issues in conference recording
Applying: FS-10802: [mod_conference] Convert conference floor to id based
Applying: FS-10891: [mod_conference] Refactor mux video to be smoother #resolve
Applying: FS-10892: [mod_av] Lip Sync Improvements #resolve
Applying: fix split slice
Applying: FS-10821: [mod_conference] fix arg parser in file vol command in conference #resolve
Applying: FS-10890: [mod_av] Wrongly calculated delta_tmp for end of video file recording #resolve
Applying: tweak av and ensure first image write at pts = 0 to avoid a black screen
Applying: FS-10840: [mod_sofia] max-registrations-per-extension parameter is not multi-tennant
Applying: FS-10243: [mod_conference] Add conference variables
Applying: FS-10854: [mod_conference] Canvas FG Image not refreshed before writing to video recordings #resolve
Applying: FS-10860: [core] Distorted music when playing it as local stream into a conference as hold music #resolve
Applying: FS-10862: [freeswitch-core] Updates on webrtc to keep up with browsers #resolve
Applying: FS-10867: [freeswitch-core] Prevent stack smash when queing multiple sound files without event-lock #resolve
Applying: FS-10749: [mod_amqp] Crash on unload after mod_amqp reloaded with  command + incorrect command behavior #resolve
Applying: FS-10865: [mod_conference] conference transfer event reports incorrect info in New-Conference-Name #resolve
Applying: FS-10871: [mod_conference] Zoomed layouts do not auto-center in mod_conference #resolve
Applying: FS-10883: [mod_conference] Conference member can get stuck read locked #resolve
Applying: FS-10893: [mod_conference] Add more banner text params #resolve
Applying: FS-10894: [Packaging-Debian] Update maintiner
Applying: FS-10896: [freeswitch-core] Parse error on originate syntax with nested square brackets #resolve
Applying: FS-10897: [mod_verto] Possible crash in verto during error condition #resolve
Applying: FS-10903: [mod_sofia,mod_valet_parking] Fix Issue with subscriptions and Valet Parking #resolve
Applying: FS-10899: [core] 3pcc-mode=proxy is proxying SDP in the 200OK with late offer invites #resolve
Applying: FS-10908: [mod_amqp] AMQP routing key (format_fields) formation broke and invalid reads reported by valgrind #resolve
Applying: FS-10904: [core] DTMF only works from one phone during shared call (SCA) #resolve
Applying: FS-10880: [mod_sofia] SIP INFO DTMF method not working on b-leg #resolve
Applying: FS-10926: [mod_sofia] fix crash from malformed as-feature-event subscribe messae with malformed xml
Applying: FS-10913: [mod_sofia] ignore_early_media=ring_ready not transitioning  #resolve
Applying: FS-10913: [mod_sofia] ignore_early_media=ring_ready not transitioning  #resolve
Applying: FS-9753: [mod_sofia] Fix crash when accessing the WSS interface via regular HTTPS
Applying: FS-11031: [mod_conference] refresh and keyframes sent too often in multi-canvas mode #resolve
Applying: FS-11036: [mod_rtmp] No audio on rtmp clients #resolve
Applying: FS-11037: [mod_lua] reduce logging levels
Applying: FS-10853: Fix unitialzed var
Applying: FS-11038: [mod_sofia] fix crash in gwlist api command
Applying: FS-11047: [Debian] re-enable mod_v8 package build
Applying: FS-11052: Allow alias for crypto suites
Applying: FS-11056: [core] fix RTCP lost calculation
Applying: FS-11057: [mod_conference] CPU race on personal canvas #resolve
Applying: Revert "FS-11047: [Debian] re-enable mod_v8 package build"
Applying: FS-11058: [core] Add RTT to RECV_RTCP_MESSAGE
Applying: FS-11057: [mod_conference] CPU race on personal canvas #resolve
Applying: FS-11063 Use compile time constants in dtls_state_setup
Applying: FS-10853: remove extern that is no longer needed
Applying: swigall
Applying: FS-11020: [Build-System] On Windows: Add missing modules to the msi installer, fix mod_gsmopen build, remove mod_skyopen, disable libav and libx264, cleanup.
Applying: FS-10989: [core] Fix CUSTOM_HASH hash table race in switch_event logic causing crashes during bind to events.
Applying: FS-11065: [Build-System, mod_v8] Update libv8 to 6.1.298 on windows, speedup windows build by moving libv8 to pre-compiled binaries.
Applying: FS-11066: [Build-System] Make WIX project be able to produce snapshots with proper msi naming on windows.
Applying: FS-11067: [Build-System] Improve windows build speed moving openssl to pre-compiled binaries.
Applying: FS-11057: [mod_conference] CPU race on personal canvas #resolve
Applying: FS-11068: [mod_conference] Avatar members not supported on personal canvas leading to miscount #resolve
Applying: FS-11057: [mod_conference] CPU race on personal canvas #resolve
Applying: revert
Applying: FS-11057: [mod_conference] CPU race on personal canvas #resolve
Applying: FS-11069: [Build-System] Update zlib to 1.2.11 and move it to pre-compiled binaries on Windows.
Applying: FS-11074: [Core, Build-System] Add PostgreSQL to the Freeswitch Core on Windows.
Applying: revert
Applying: FS-11057: [mod_conference] CPU race on personal canvas #resolve
Applying: FS-11075: [mod_amqp] Add mod_amqp to the Windows build.
Applying: FS-11076: [mod_cdr_pg_csv] Add mod_cdr_pg_csv to the Windows build.
Applying: FS-11070: [mod_conference] Improve video bridge first two for mux mode -- add support for files playing in this mode #resolve
Applying: FS-11078: [Build-System] Add mod_say_(es_ar, fa, he, hr, hu, ja, pl, th) modules to the Windows build.
Applying: Revert "FS-11070: [mod_conference] Improve video bridge first two for mux mode -- add support for files playing in this mode #resolve"
Applying: Revert "FS-11057: [mod_conference] CPU race on personal canvas #resolve"
Applying: rewind
Applying: rewind
Applying: FS-10883: [mod_conference] Conference member can get stuck read locked #resolve
Applying: tmp revert
Applying: FS-10893: [mod_conference] Add more banner text params #resolve
Applying: FS-10967: [mod_conference] chan var "conference_enter_sound" not working, only work when profile param active and being overridden #resolve
Applying: FS-11017: [mod_conference] Add moh controls to conference #resolve
Applying: FS-11021: [mod_conference] Add video mirror #resolve
Applying: FS-11031: [mod_conference] refresh and keyframes sent too often in multi-canvas mode #resolve
Applying: FS-11034: [mod_conference] Add border control to video #resolve
Applying: FS-11057: [mod_conference] CPU race on personal canvas #resolve
Applying: FS-11068: [mod_conference] Avatar members not supported on personal canvas leading to miscount #resolve
Applying: FS-11070: [mod_conference] Improve video bridge first two for mux mode #resolve
Applying: FS-11070: [mod_conference] Improve video bridge first two for mux mode -- add support for files playing in this mode #resolve
Applying: FS-10980: [mod_lua] Update lua version to 5.3.4 and move it to pre-compiled binaries on Windows.
Applying: FS-11082: [Build-System] Add mod_odbc_cdr to the Windows build.
Applying: FS-11083: [Build-System] Add mod_cdr_sqlite to the Windows build.
Applying: FS-11085: [Build-System] Update curl to 7.59.0 and move to pre-compiled binaries on Windows.
Applying: FS-11086: [Build-System] Move libflite to pre-compiled libraries on Windows.
Applying: FS-11024 #resolve fix pdf turn pages
Applying: FS-11022 #resolve ImageMagic 7 support
Applying: FS-11077: [mod_enum] Memory Leak in mod_enum #resolve
Applying: FS-11080: [freeswitch-core] Auto sync of jb can fail on extreme loss #resolve
Applying: FS-11081: [mod_video_filter] Teleportation #resolve
Applying: FS-11014 [core] add vad to core
Applying: update adapter.js
Applying: FS-10945: [Build-System] build fails for Master  #resolve
Applying: FS-11014: [core] Add vad to core on Windows.
Applying: FS-10908: [mod_amqp] AMQP routing key (format_fields) formation broke and invalid reads reported by valgrind #resolve
Applying: FS-10941: [mod_conference] Segfault SIGFPE, Arithmetic exception in mod_conference #resolve
Applying: FS-11016: [mod_av] Looping a video file that has resize feature enabled can allow a non-resized frame to slip after file seek #resolve
Applying: FS-10918 #resolve
Applying: FS-10946: [core] Duplicate code in switch_xml.c #resolve
Applying: [mod_shout] use 0xFFFFFFFF flag to query frame_size
Applying: swigall
Applying: FS-11087: [Build-System] Use shared zlib library on Windows.
Applying: FS-11090: [Build-System] Move PCRE library to pre-compiled binaries, minor cleanup, reduce build log verbosity on windows.
Applying: FS-11091: [mod_sndfile] Remove libsndfile from the code base, use pre-compiled binaries on Windows, update to libsndfile 1.0.28
Applying: FS-11097: [mod_cv] Add OpenCV 3.x support.
Applying: FS-10987: [mod_conference] fix member deadlock on write failure
Applying: FS-10997: [libvpx] CVE-2017-13194
Applying: FS-10998: [libvpx] CVE-2017-0641
Applying: FS-11055: [mod_av] resize image to recording image size if it does not match recording size
Applying: FS-11101: [mod_cv] Add mod_cv to the Windows build.
Applying: FS-10867: [freeswitch-core] fix regression in stack smash protection
Applying: FS-11109 #resove tweak bash rc
Applying: [FS-11092] #resolve add cJSON_isTrue
Applying: FS-11111 #resolve add handle params to specify tts engine and voice from command text
Applying: FS-11110 #resolve resume detect on next detect
Applying: FS-11093 #resolve switch_true should return switch_bool_t
Applying: FS-11122: [Build-System] Fix improper use of debug symbols settings on Windows.
Applying: FS-10756: Removed all windows related libsodium stuff, no longer using it at all
Applying: remove some unused files
Applying: FS-11125: [Build-System] Remove unused files left from previous Visual Studio version.
Applying: FS-11138: [freeswitch-core] Leak in ESL client #resolve
Applying: FS-11127: [freeswitch-core] Improvements to Video JB and audio jb sync #resolve
Applying: FS-10892: [mod_av] Lip Sync Improvements -- increase delta #resolve
Applying: FS-10972: [Verto-Communicator] add (-y) parameter to the verto install script debian8-install.sh #resolve
Applying: FS-11047: add mod_v8 back to build
Applying: FS-11104: [build] use freeswitch custom ldns package if available
Applying: FS-11104: [build] use the right openssl version everywhere
Applying: FS-11104: [build] use freeswitch custom ldns package if available
Applying: FS-11047: [build] detect v8.pc as well
Applying: FS-11047: [build] support v8 6.6 fixes from Andrey Volk
Applying: Revert "FS-11048: [build] support v8 6.6 fixes from Andrey Volk"
Applying: FS-11047: [build] support v8 6.6 fixes from Andrey Volk
Applying: Revert "FS-11052: Allow alias for crypto suites"
Applying: FS-11119: [core] Fix video skew on oddly resized conference layers
Applying: FS-11150: [Build-System] Fix broken ESL in .Net project.
Applying: FS-11156: [mod_dptools] wait for ack in application "deflect" #resolve
Applying: FS-11162: [zrtp] Hangup race causing rare crash on zrtp calls
Applying: FS-11164: [freeswitch-core] Improve audio JB in bad conditions #resolve
Applying: FS-11165: [freeswitch-core] Add more regex handling to dmachine #resolve
Applying: FS-11165: [freeswitch-core] Add more regex handling to dmachine -- squashme #resolve
Applying: FS-11164: [freeswitch-core] Improve audio JB in bad conditions
Applying: FS-11173: [mod_conference] Don't send tmmbr for any higher than input res #resolve
Applying: FS-11177: [freeswitch-core] h264 vid tweaks #resolve
Applying: FS-11178: [core] return switch_status_t from switch_ivr_intercept_session
Applying: FS-11177: [freeswitch-core] h264 vid tweaks -- increase gop #resolve
Applying: reswig
Applying: reswig
Applying: FS-11201 Filter out erroneous RTT values #fix
Applying: FS-11202 Add sanity check
Applying: FS-11208: [Debian] Prefer libssl1.0 in deb packages
Applying: FS-11209: [Debian] openssl linking
Applying: FS-11209: [Debian] openssl linking
Applying: FS-10949: [mod_conference] allow vblind vunblind tvblind commands when caller is not sending video
Applying: FS-11222: [core] NACK for multiple packets sends wrong packet after the first one
Applying: FS-11223: [core] fix Crash when firefox sends only rtcp and not rtp candidates on video media
Applying: FS-11238: [mod_conference] Regression from FS-10448 can cause stuck sessions #resolve
Applying: FS-11263: [Build-System] Move FreeSWITCH build system to Visual Studio 2017 on Windows.
Applying: FS-11211: [Verto-Communicator] Add turnServer and verto server fallback options -- FS side to only do relay as a last resort #resolve
Applying: FS-11224: [freeswitch-core] Fix VC build #resolve
Applying: FS-11225: [freeswitch-core] Crash in fs_cli #resolve
Applying: FS-11226: [freeswitch-core,mod_conference] Missing font files can lead to crash in conference #resolve
Applying: FS-11231 #resolve deprecate start_input_timers and add start-input-timers
Applying: FS-11183 improve strip to save out buffer size
Applying: FS-11206: [mod_conference] add conference hold feature
Applying: FS-11189: [core] add func to parse cpu string
Applying: FS-11189: add vpx settings
Applying: FS-11189 add vpx config
Applying: FS-11189 tweak log
Applying: FS-11189 add vpx codec specific and debug controls
Applying: FS-11189 refactor and add more params
Applying: FS-11189 make rtp-slice-size and key-frame-min-freq configurable
Applying: FS-11189 add avcodec settings
Applying: FS-11189 refactor to support any possible codec specific private options
Applying: FS-11237 #resolve speak text with colon
Applying: FS-11259: [mod_perl] mod_perl tweaks #resolve
Applying: FS-11014 fix fvad_free and validate vad_mode
Applying: FS-11264: [freeswitch-core] Remove old speech handle when asr failure happens #resolve
Applying: FS-11260 fix set_tts_params segs when the second arg is NULL
Applying: FS-11189: [mod_av] fix build on older libav
Applying: version bump
Applying: FS-11189: [Build-System] Fix Windows build.
Applying: FS-11189: [Build-System] Fix Windows build.
Using index info to reconstruct a base tree...
M       src/include/switch_utils.h
Falling back to patching base and 3-way merge...
No changes -- Patch already applied.
Applying: FS-11189: [Build-System] Windows build regression fix.
Applying: FS-11189: [Build-System] Windows build regression fix.
Using index info to reconstruct a base tree...
M       src/include/switch_utils.h
Falling back to patching base and 3-way merge...
No changes -- Patch already applied.
Applying: version bump
Applying: FS-9258: Recursive calls to $.verto.rpcClient.speedTest() don't work
Applying: FS-11149: Playing video files assigned a res_id not correctly updated on layout change
Applying: FS-11117: font_scale float values, add min/max_font_size when setting video banner
Applying: FS-10554: Refactor conference vid-res-id command, add 'force' option
Applying: FS-10450 #resolve fix zero and negative file duration
Applying: FS-11276: dedicated video layers can no longer become audio floor holder
Applying: FS-11278 #resolve fix if clause does not guard compiler warning
Applying: FS-11280: Allow overriding permissionCallback per Verto dialog
Applying: FS-11281: Verto.newCall dialog callback overrides should be set before invite
Applying: FS-11282: Remove iOS 'controls' hack, not needed
Applying: FS-11283: iOS doesn't support beforeunload, use recommended pagehide for that platform
Applying: FS-11284: Fix legacy/broken video constraints for specifying a video deviceId
Applying: FS-11285: OverconstrainedError on some Android front cameras when frameRate.min is specified
Applying: FS-11286: Add onRemoteStream callback to Verto dialogs
Using index info to reconstruct a base tree...
M       html5/verto/js/src/jquery.FSRTC.js
M       html5/verto/js/src/jquery.verto.js
Falling back to patching base and 3-way merge...
Auto-merging html5/verto/js/src/jquery.verto.js
Auto-merging html5/verto/js/src/jquery.FSRTC.js
Applying: FS-11287: Provide option for user managed streams in Verto
Applying: FS-11288: [Build-System] Refactor WIX project to not miss modules in msi on slow machines. Move Windows build logic from legacy util.vbs to modern props. Fix sound packages improper extraction. Fix multiprocessor build under msbuild.
Applying: FS-11294: [mod_av] Fix mod_av build on Windows.
Applying: FS-11295: [mod_av] Fix wrong path to yasm.exe
Applying: FS-11297: [Build-System] Add mod_cidlookup to the Windows build.
Applying: FS-11298: [Build-System] Fix Win32 build under Visual Studio.
Applying: FS-11271: [Build-System] Add mod_cdr_mongodb to the Windows build.
Applying: FS-11303: [Build-System] Migrate Visual C++ components redistribution using merge modules from v140 to v141 (VS2017), minor cleanup.
Applying: FS-11310 #resolve add optional switch_core_file_pre_close() to stop writing to file and possible to get file size related params
Applying: FS-11310 #resolve add more params for conference record stop event
Applying: FS-11316 [mod_rayo] Add dialplan app execution component
Applying: FS-11316 [mod_rayo] Add FS API support
Applying: FS-11328: [mod_lua] Update Lua version from 5.3.4 to 5.3.5 for the Windows build.
Applying: FS-11333: [mod_mp4v2] improvements
Applying: FS-11154: [freeswitch-core] Improve audio sync during loss #resolve
Applying: FS-11110 fix for resume detect on next detect
Applying: FS-11206: [mod_conference] add conference hold feature
Applying: FS-11265 #resolve add detectSpeech and playAndDetectSpeech
Applying: FS-11320 #resolve pass json string as param to ASR module
Applying: FS-11321: [mod_conference] Don't set avatar when its not allowed anyway #resolve
Applying: FS-11322: [freeswitch-core] Change ice handling to work with FireFox when in turn mode #resolve
Applying: FS-11304 - [Configuration]: update vanilla config files for correcting conference_utils_auto_outcall_flags
Applying: FS-11052: Allow alias for crypto suites
Applying: FS-11335: [mod_curl] Update cURL version to 7.61.0 for the Windows build.
Applying: FS-11340: [freeswitch-core] Add some more code to vad obj to enable debuging and manage states better #resolve
Applying: FS-11341: [core] clang static analyzer warning in switch_utils.h:switch_parse_cpu_string
Applying: FS-11346: [core] add api to pass pre-parsed values instead of dial strings to switch_ivr_originate
Applying: swigall
Applying: FS-11279: Wrap verto.clientReady message callback in check for onMessage callback function
Applying: FS-11308: fix segfault on invalid multipart sdp
Applying: FS-11164 fix unused function regression from 578d914b9
Applying: FS-11189 use AV_INPUT_BUFFER_PADDING_SIZE instead of FF_INPUT_BUFFER_PADDING_SIZE in newer ffmpeg
Applying: FS-11189 add default av conf to vanilla
Applying: FS-11189 add helper functions and macros to dump encoder context params
Applying: FS-11189 set default cpu string to cpu/2/4 if not configured
Applying: FS-11189 add back default flags LOOP_FILTER and PSNR for H264
Applying: FS-11351: [swig] fix windows build with swig3 for mod_managed and fix make swigall
Applying: FS-11189 add H264 default private configs
Applying: FS-11189: [mod_av] fix build on older libav
Applying: FS-11361: [Build-System] Switch Debian packages building script util.sh from jessie stable to stretch stable.
Applying: FS-11368: [mod_flite] Use system libflite.
Applying: FS-11368: [mod_flite] Use system flite1 instead of libflite
Applying: FS-11372: [mod_commands] garbage uuid on output from bgapi  #resolve
Applying: FS-11360 Fix FS degradation over time in DTLS layer (especially if outdoing packets rate higher that incoming)
Applying: FS-11362 FS could close client verto connection due to incorrect handling of SSL function return values (when SSL layer need to communicate with client on its own, f.e. keys re-negotiation)
Applying: FS-11362 Rearrange poll() errors handling to properly report poll hangup. Handle and log case when client sent close request. Add errno to errors where applicable.
Applying: FS-11201 Fix 'rtt_valid = 0;' was incorrectly placed rendering whole RTT thing void.
Applying: FS-11364 Fix tile flicker when layout has 'overlap' and 'zoom' options
Applying: FS-11368: [mod_flite] Use system libflite.
Applying: FS-11362 Small macro tune based on James Dictos comment
Applying: Revert "FS-11209: [Debian] openssl linking"
Applying: FS-11206: [mod_conference] rework behaviors of conference hold to still toggle states while on hold
Applying: FS-11363 Fix unpredictable/random zoom, when camera's video aspect ratio (a/r) equal to conference layout's tile a/r
Applying: FS-11287: [verto] update verto-min.js
Applying: FS-10723: [mod_conference] Add event generation when video feed interrupted occurs
Applying: FS-11349: [mod_av] remove debug logging
Applying: FS-11349: [mod_av] remove debug logging
Using index info to reconstruct a base tree...
M       src/mod/applications/mod_av/avcodec.c
Falling back to patching base and 3-way merge...
No changes -- Patch already applied.
Applying: FS-11377 [freeswitch-core] lock/unlock mutexes in consistent order.
Applying: FS-11380: [core] add new internal function
Applying: FS-11379: [freeswitch-core] Rare race condition in state machine #resolve
Applying: FS-10827 [mod_spandsp] Make thread safe and xmlreload do not affect already started detection
Applying: FS-11189 Rearrange VPX code to fix few bugs and make it more structured.
Applying: FS-11189 add some default config to be consistent if default xml config is missing
Applying: FS-11189 do not allow user change of g_timebase
Applying: FS-11362 Fix die_errnof() macro failing to compile on mac
Applying: FS-11382: [core] add switch_pool_strip_whitespace function to strip whitespace using pool allocation
Applying: swigall
Applying: FS-11387 [mod_loopback] add null endpoint
Applying: FS-11362: [mod_verto] Fix regression for the broken Windows build.
Applying: FS-11225: [freeswitch-core] Crash in fs_cli -- missing check for null pointer #resolve
Applying: FS-11390: [mod_codec2] Use system libcodec2.
Applying: FS-11394 reenable avcodec nvenc support
Applying: FS-11394: [mod_av] Enable HW acceleration on Windows. Use FFmpeg 3.4.4 instead of libav.
Applying: FS-11393: [core] set record_trimmed_ms at the end of record_session if the file interface has this info available
Applying: FS-11394: [mod_av] Fix "restrict" regression.
Applying: FS-11396: [mod_v8] Use proper libv8 naming.
Applying: FS-11397 [mod_rayo] add xmlns to exec <response> to make adhearsion's life easier.
Applying: FS-11398: [Build-System] Disable mod_event_zmq debian package.
Applying: FS-11393: [core] set record_record_trimmed_ms at the end of record app if the file interface has this info available
Applying: FS-11400: [Build-System] FreeSWITCH Sound packages on Windows.
Applying: FS-11394: [mod_av] Fix Windows build.
Applying: FS-11403: [Build-System] Add missing htdocs, images, fonts, grammar to the Windows installer. Add descriptions to the installed components. Cleanup legacy project files.
Applying: FS-11014 FS-11404 fix libfvad might be enabled unexpectedly when set_mode is called
Applying: FS-11407: [freeswitch-core] process media bugs in the order they were added #resolve
Applying: FS-11405 #resolve tweak macro to use do while 0
Applying: FS-11406 #resolve allow displace session replace both read and write audio
Applying: FS-11340 export switch_vad_state2str and update vad_test to support the latest params
Applying: FS-11340: [core] fix merge conflict
Applying: FS-11409: add "ice-lite" SDP attribute
Applying: FS-11412: [core] Fix crash caused by missing or malformed ice candidates in sdp
Applying: version bump
Applying: FS-11416 [core] add test framework header files.
Applying: FS-11422: [mod_java] Fix logging.
Applying: FS-11423: [mod_isac] Add ARM64 detection.
Applying: FS-10745 [mod_sofia] provide more context to transferor event
Applying: FS-11207: [core] Fix msrp_init_ssl error handling
Applying: FS-11444: Capture return code from `os.execute()`; stop attempting to concat boolean/nil to error string.
Applying: FS-11207: [core] Fix msrp_init_ssl and msrp_deinit_ssl functions, check globals.ssl_ready variable
Applying: FS-11445: [Build-System] Add distro to the name of the source tarball.
Applying: add freeswitch-mod-shout as dep
Applying: fix mod_hiredis deps
Applying: Fix FS-9865
Applying: FS-11312 [core] prevent double firing record_start event
Applying: FS-11470: WebRTC Unified plan not properly supported
Applying: FS-11472: Remove Android frameRate.min OverconstrainedError hack
Applying: FS-11471: [mod_sofia] revert changes to send PAI for polycom 4.x versions differently, seems polycom has now fixed this
Applying: FS-11485: Debian systemd service file out of date
Applying: FS-11450 [mod_conference] Fix for memory leak when conference json list api command is executed
Applying: FS-11494: [Debian] force build of iksemel before modules to avoid -j race condition on dep lib build
Applying: [core] Test commit- bump copyright date.
Applying: FS-11506: [mod_sofia] Handle multiple History-Info headers in MESSAGE
Applying: [FS-11507] use explicit architecture in control file
Applying: FS-11508: [mod_sofia] Make max-registrations-per-extension use extension instead of username
Applying: Revert "Merge pull request #1608 in FS/freeswitch from ~HUNYI/freeswitch:bugfix/FS-11207-sofia-msrp_worker-crash-when-initializing to master"
Applying: FS-11207: [core] msrp: fix init ssl
Applying: FS-11118 #resolve mod_sofia send display update to Grandstream family user-agent
Applying: FS-11520: [mod_curl] Make max_bytes configurable
Applying: FS-11530: [mod_lua] add configure support for LuaJIT
Applying: FS-11413: [freeswitch-core,mod_conference,mod_local_stream] High memory footprint in some video situations #resolve
Applying: FS-11394: [mod_av] Clean up defaults cos they are still wrong
Applying: FS-11394: [mod_av] Fix cpu race created by this patch
Applying: FS-11466: [mod_av] Fix h264 hardware encoding delay.
Applying: FS-11523: [mod_av] Add ffmpeg 4.1 support.
Applying: FS-11464: [Build-System] Improve CUDA detection in Visual Studio on Windows.
Applying: FS-11539: [core] session message names print wrong in log messages
Applying: FS-11512 [mod_sofia] #resolve missing privacy header on display update
Applying: FS-11554: fix crash in conference API when no param given to "moh".
Applying: FS-11562 #resolve add register ip and port to gateway register state change events
Applying: FS-11532: fix crash (87d4a6a0)
Applying: FS-11562 [mod_sofia] fire register state event on changing registrar IP
Applying: FS-11345: Fixed Werror=stringop-truncation for mod_opus
Applying: FS-11345: Fixed Werror=format-truncation on lib/esl
Applying: FS-11559: check file name null ptr (crashfix CoreSession:recordFile)
Applying: FS-11416 fix build error FCT_FIXTURE_SUITE_BGN undefined
Applying: FS-11439 [core] Update switch_test.h so unprivileged users can execute tests when basedir is owned by root.
Applying: FS-11442 [core] added fst_play_and_detect_app_test and fst_sched_recv_dtmf
Applying: FS-11442 [core] added test helpers for constructing XML objects
Applying: FS-11442 [core] Add test helpers for wall time measurements and integer/double range checks.  Improve simple test log output to identify which test the failure is in.
Applying: FS-11442 [core] allow multiple test modules to be loaded in the same test program.
Applying: FS-11442 [mod_test] new module, mod_test - mock ASR interface.
Applying: FS-11453 [mod_rayo] add support for FS JSON ASR result.
Applying: FS-11453 [mod_rayo] allow text/plain grammar for non-mrcp speech recognizers.  Added direction to output component to assist with testing.  Fix confidence conversion to NLSML.
Applying: FS-11453 [mod_rayo] convert to fst testing
Applying: FS-11455 #resolve add originate test for state handlers
Applying: FS-11453 [core] remove dependency to libtap for unit tests
Applying: FS-11461: [core] Fix mod_av test code on Windows.
Applying: FS-11461: [mod_av] Add test code, improve tests on Windows.
Applying: missing comma
Applying: FS-11487 #resolve convert to Makefile.am
Applying: FS-11442 [test] Fix double-free on test suite cleanup.
Applying: FS-11442 [test] add fst_check_extension_apps to compare dialplan apps with expected.
Applying: FS-11442 [test] enable module runtime when loading it
Applying: FS-11455 add switch_utils test
Applying: FS-11505: [core] System call with output capture on Linux and Windows without fork. Add switch_core test.
Applying: FS-11569 add rate to null endpoint
Applying: FS-11487 convert tests to Makefile.am
Applying: FS-11547: [core] Fix broken record_sample_rate variable.
Applying: FS-11565: [Build-System] Add 48kHz sounds to the Visual Studio solution on Windows.
Applying: FS-11555: [mod_signalwire] Initial commit.
Applying: version bump

A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git branch
* (HEAD detached from refs/heads/v1.8)
  master
  v1.6
  v1.6_new
  v1.8
  v1.8_new

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git checkout v1.8_new
Warning: you are leaving 441 commits behind, not connected to
any of your branches:

  90c2a2afbd version bump
  0eb559c156 FS-11555: [mod_signalwire] Initial commit.
  4a411a36e4 FS-11565: [Build-System] Add 48kHz sounds to the Visual Studio solution on Windows.
  e3a67793fc FS-11547: [core] Fix broken record_sample_rate variable.
 ... and 437 more.

If you want to keep them by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> 90c2a2afbd

Switched to branch 'v1.8_new'

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git branch
  master
  v1.6
  v1.6_new
  v1.8
* v1.8_new

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git reset --hard 90c2a2afbd
HEAD is now at 90c2a2afbd version bump

```

the v1.6_new branch fork log:
```

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase 16d600c0350a79c2532c739dd1432f7ed318ea09^ 43a9feb7f8705af5008c963e36e36b9b1d69c373 --onto v1.6_new
First, rewinding head to replay your work on top of it...
Generating patches: 100% (1714/1714), done.
Applying: FS-7486: Fix handling of queued requests in Sofia-SIP
Applying: Making mod_rtmp compatible with Adobe Media Server
Applying: FS-7997 #resolve We need to have a designer look at the html partial and come up with a CSS to make this better looking.
Using index info to reconstruct a base tree...
A       html5/verto/verto_communicator/partials/chat.html
Falling back to patching base and 3-way merge...
Auto-merging html5/verto/verto_communicator/src/partials/chat.html
Applying: FS-7344: Fix race condition for INVITE right after ACK in 3PCC mode.
Applying: bump versions numbers in prep for initial 1.6 release
Applying: FS-1772 #resolve fix reset of voicemail greeting to default
Applying: FS-8131: [mod_voicemail] fix disallowed empty password set
Applying: explore use of nuget for wix build dependency
Applying: FS-7168 update Debian packages so that FS core libraries are setup as runtime deps
Applying: FS-7697 chown the /etc/freeswitch/tls directory so that the freeswitch user can have read/write for TLS certificate generation
Applying: FS-8136: [mod_h26x] do not load passthru video codecs by default
Applying: FS-8141 Add support for apr_queue_term() to switch_apr.c
Applying: FS-8140 Fix user_name typo in sofia_handle_sip_i_invite
Applying: FS-8142 Fix a thread cache thread-safety and caching
Applying: make the changes for the 1.6 repos in the repo install rpm spec file and repo file
Applying: FS-8072 #resolve fix a missed space
Applying: FS-8075 fix typo for dialplan app
Applying: FS-8142 minor formatting mod
Applying: FS-7486 #resolve update sofia
Applying: FS-8075 Fix for failover when you pull power on redis, while redis clients under load test
Applying: FS-8144 readability and code formatting cleanup of mod_opus whiel reviewing PLC/FEC bug and document missing options from opus.conf.xml
Applying: FS-8126 #resolve [Pruning of a media bug may cause all media bugs on a session to be pruned]
Applying: FS-1772 build err
Applying: FS-8034: mod_opus: send correct (configured) fmtp ptime,minptime,maxptime when originating call
Applying: FS-8143 #resolve #comment [mod_rayo] Fix crash caused by client disconnecting from mod_rayo while a message is being delivered to that client.
Applying: libs/spandsp/.gitignore: added temporary files in test-data
Applying: FS-7967 SmartOS compatibility
Applying: FS-8053: handle a=sendonly, a=sendrecv, a=recvonly to change who is sending video during a call
Applying: Fix process spawing segfault
Applying: FS-8149: fix mod_xml_cdr curl dependency in makefile
Applying: FS-8130 don't set packet loss percent
Applying: FS-8130
Applying: swigall
Applying: FS-6833 FS-6834 fix double re-invite on media establishment
Applying: FS-8053 fix some regressions from original merge, add auto mute-unmute when toggling video send/recv
Applying: FS-8160: properly handle malformed json when parsing json with \u at the end of a json string
Applying: down to 512 instead of 256
Applying: FS-8114 #resolve [Opus and telephone event payload types collide on REFER]
Applying: typo
Applying: FS-8053 addtl touchups
Applying: FS-8165 #resolve Fix debian build depends for mod_hiredis
Applying: FS-8130 add support for timestamp based counting for jitter buffer in audio mode
Applying: Fix debian/bootstrap.sh from whitespace error
Applying: Cleanup non-semantic whitespace in debian/
Applying: FS-8130 more cleanup on jb mods
Applying: FS-8130 FS-7432 FS-8115
Applying: FS-8169 #resolve [uuid_displace on stereo channels can lead to memory corruption causing a crash]
Applying: FS-8167 #resolve [FS segs]
Applying: FS-8130 fix regression in conference recording
Applying: FS-8130 redo last fix a different way
Applying: FS-8172 #resolve [Regression from earlier commit to mod_conference breaks admin controls in verto demo app]
Applying: FS-6833 FS-6834 add support for X-headers in this 3p mode
Applying: FS-8130 code error
Applying: FS-8175 #resolve [Add continue_on_answer_timeout variable to allow channel to proceed from a tripped answer timeout]
Applying: FS-8173 fix SAVPF printing when it's really AVPF
Applying: FS-8080: mod_opus: fine-tune FEC with repacketization (ptimes: 80 ms,100 ms,120 ms)
Applying: FS-8178 - [verto_communicator] Define first share device selected by default in settings modal
Applying: FS-8078 - [verto_communicator] fix display flex in safari
Applying: FS-8179: mod_opus:  improve JB debugging with FEC
Applying: FS-8180 #resolve [param to enable/disable video malfunction]
Applying: FS-8180 addtl patch
Applying: FS-8161: mod_opus: Keep FEC enabled only if loss > 10 ( otherwise PLC is supposed to be better)
Applying: FS-8180 typo, sigh
Applying: FS-8155 - [verto_communicator] check for nulled data.liveArray preventing locked video interface
Applying: FS-8180 fixed for real
Applying: FS-5660 - add freeswitch.py to the freeswitch-mod-python debian package
Applying: FS-8146 - [verto_communicator] fix loong names in members list
Applying: FS-8181 #resolve [Verto check for camera perms fails init when no camera is present]
Applying: FS-8130 cont
Applying: FS-8130 fix regression causing seg
Applying: FS-8184 #resolve [Fix possible memory leak in mod_conference when hanging up on a video call]
Applying: FS-8185: [core] Allow xml preprocessor to expand variables where the resulting value is much longer than the original size
Applying: FS-8130 more refactoring
Applying: FS-8130 don't skip delta on video unless it was less than 1 second worth to prevent locking up chrome
Applying: FS-8130 the saga continues
Applying: Revert "FS-8078 - [verto_communicator] fix display flex in safari"
Applying: FS-8078 #resolve fix safari without breaking firefox
Applying: fix small typo in js and regen minified js
Applying: FS-8130 the bug that keeps on giving
Applying: FS-8130 the bug that keeps on bugging, improve video nack
Applying: FS-8130 amend last commit
Applying: FS-8193 #resolve Add No Camera option to Verto Communicator
Applying: FS-8095 [verto_communicator] added reset button to default settings.
Applying: FS-8042, FS-8182: add ping time (in ms) to sip_registrations table, displays as part of the show commands that show registration details, add force_ping=true user var to force options ping on individual registered endpoints
Applying: FS-8196 [verto_communicator] Fix sidebar layout when user is moderator.
Applying: FS-8196 [verto_communicator] adding nowrap to white-space.
Applying: FS-8130 running out of witty commit msgs
Applying: FS-8130 refactor pass
Applying: FS-8095 [verto_communicator] Do not reset name and email upon logout.
Applying: FS-8102 #resolve #comment Add auto-provision/config support to VC
Applying: FS-8183 #resolve #comment add google api logins to Verto Communicator
Applying: remove unused code
Applying: FS-8190 #resolve [When using nixevent, freeswitch stops sending us certain custom event that were NOT part of the nixevent command]
Applying: FS-8188 add park_after_early_bridge transfer_after_early_bridge variables
Applying: FS-7673: ODBC NULL value incorrectly evaluated in mod_v8
Applying: Use systemd RuntimeDirectory for /run/freeswitch
Applying: Move chown of /etc/freeswitch/tls to postinst
Applying: Remove explicit set of WorkingDirectory
Applying: FS-8089 [verto_communicator] Opening members list when start conference
Applying: FS-8095 [verto_communicator] Moving factory reset button to bottom left with red color and confirmation. Reloading page upon reset.
Applying: FS-8198: Fixed default CRLF sequence in t38 SDP
Applying: FS-8202 #resolve [Stop peer when ending screen share]
Applying: FS-8031 firefox gives up very fast, autoadj on any valid packet when dtls is not up
Applying: FS-8031 firefox was giving up because we replied to their stun
Applying: FS-8204 #resolve [Add stereo audio support for opus on FireFox]
Applying: FS-7980 [verto_communicator] Fixing video padding-top.
Applying: FS-8204 add sprop-stereo also
Applying: FS-8200 [verto_communicator] Ending screenshare if user hangup call. Screenshare now is for all instead of only mods.
Applying: FS-8130 add jb_video_low_bitrate for a bit rate to ask for when the jb is taxed
Applying: FS-8183 tweak the CSS to look better
Applying: FS-8210 #resolve [mod_verto can be unloaded while it is in use]
Applying: correct version of proposed patch
Applying: FS-8211 #resolve [Conference video recordings of layouts with overlap have flickering video]
Applying: FS-8130 regression in bridged channels with jitterbuffers
Applying: FS-8190: fix build regression from original FS-8190 commit
Applying: FS-8215 #resolve [MacOSX nanosleep is not super accurate]
Applying: FS-8183 Move google ClientID to config file where it belongs
Applying: FS-8216 #resolve [Occasional lip sync problems when recording with mod_av ]
Applying: FS-7911 #resolve
Applying: FSRTC - calling localStream.stop when it's available or localStream.active = false to make it work on newer chrome canary
Applying: bump version
Applying: fix identation
Applying: FS-8205 [verto_communicator] Add splash screen feature
Applying: FS-8205 [verto_communicator] fix config reload
Applying: FS-8205 [verto_communicator] fix refresh video callback
Applying: FS-8205 [verto_communicator] fix login config and checkConfig
Applying: FS-8205 [verto_communicator] fix again
Applying: FS-8205 [verto_communicator] fix incall redirect and first login (localstorage clear)
Applying: FS-8205 [verto_communicator] google client id changes
Applying: FS-8205 [verto_communicator] fix login with config provision
Applying: FS-8205 [verto_communicator] fix login
Applying: FS-8205 [verto_communicator] fix login config provision
Applying: FS-8205 [verto_communicator] fix auto call config provision
Applying: FS-8224 Allow CallerID to be set from login settings in VC
Applying: FS-8213 #resolve [Add support to skip permission checks on verto]
Applying: FS-8221 [verto_communicator] Fix number in call history
Applying: FS-8223 [verto_communicator] Fixing members list layout when callerid is too long.
Applying: FS-8223 [verto_communicator] needs display inline-block and obviously the class in the element.
Applying: FS-8225 [verto_communicator] Avoid duplicate members when recovering calls.
Applying: FS-8214 [verto_communicator] Better handling calls in VC, answering them respecting useVideo param
Applying: FS-8224 missed a field type
Applying: Bump Version Number
Applying: Removed use of _NONSTD for Windows builds of spandsp, so (hopefully) eliminate
Applying: FS-8229 [verto_communicator] Changing moderator actions bullet menu color to #333
Applying: FS-8166 #resolve [Mute/unmute while shout is playing audio fails because the channel "has a media bug, hard mute not allowed"]
Applying: FS-7989 escape double quotes from summary
Applying: FS-8220 #resolve [DTMF not working between telephone-event/48000 A leg and telephone-event/8000 B leg]
Applying: FS-8232 #resolve [conference sending too many video refresh req]
Applying: FS-8236: fix build without libyuv on compilers that error on unused static function
Applying: FS-8219 #resolve Correctly stop the tracks.
Applying: FS-8240 #resolve [local_stream a/v gets out of sync when running in the background]
Applying: FS-8241 #resolve [Conference stops playing video when local_stream changes source]
Applying: FS-7989 add more debugging data
Applying: FS-8243: Improve the way FEC info is detected within frames (added support for ptimes higher than 20 ms for FEC detection )
Applying: FS-8240 more
Applying: FS-8240 more
Applying: new 1x2-presenter-zoom layout
Applying: Testing config tweaks
Applying: FS-8219 missed a spot
Applying: FS-8239 fix default value to avoid failed mod_av build on CentOS 7
Applying: FS-8245 #resolve [Video Resolutions available in "Video Quality" drop down are not always correct]
Applying: FS-8240 finishing touches
Applying: FS-8246: use seconds as default value for delay param
Applying: FS-8248 put python binaries into site arch path
Applying: fix 2x1-presenter-zoom layout
Applying: FS-8240 add video profile param for recording 264 and make it default
Applying: set level to 4.1 on voip
Applying: FS-8251 #resolve [verto_communicator] factory reset now clear all local storage
Applying: FS-8252 #resolve [Crash in rtp stack on dtls pointer]
Applying: FS-8254 #resolve [verto_communicator] create source map file
Applying: FS-8255 #resolve fix debian codename changes since jessie was released as stable
Applying: FS-8255 #resolve fix debian codename changes since jessie was released as stable
Using index info to reconstruct a base tree...
M       debian/util.sh
Falling back to patching base and 3-way merge...
No changes -- Patch already applied.
Applying: FS-8256: mod_opus: more FMTP cleanup
Applying: FS-8257 #resolve [verto_communicator] fix config provision url
Applying: FS-8260 #resolve [verto_communicator] prompt banner text
Applying: FS-8261 #resolve
Applying: FS-8236: revert previous bad commit
Applying: FS-8236: fix ifdefs for building without libyuv
Applying: FS-8240 #comment please test
Applying: revert
Applying: FS-8216 #comment please test
Applying: FS-8263 #resolve [verto_communicator] created reset banner action
Applying: FS-8216 fix regression in hup_local_stream from last commit
Applying: FS-8273 #resolve clear the CF_RECOVERING flag in a spot that was missed
Applying: FS-8274 Fix memory leak caused by images not being freed in video_thread_run
Applying: FS-8179 #resolve [mod_opus: improvement on new JB buffer debugging (debug lookahead FEC)]
Applying: FS-8179 missing semicolon
Applying: FS-8275 #resolve [RFC2833 DTMF broken in recent master]
Applying: FS-8271 simplify package building for the default case
Applying: FS-8263 #resolve [verto_communicator] floor and presenter badges
Applying: FS-8263 $resolve [verto_communicator] lock icon in floorLocked status
Applying: FS-8282 #resolve [sleep is not interrupted by uuid_transfer]
Applying: Handle RTP Contributing Source Identifiers (CSRC)
Applying: FS-8270 #resolve
Applying: FS-8285
Applying: Use use-dtx setting from config in request to callee.
Applying: FS-8286: Minor debug log level tweaks
Applying: add alternat file option to hashfinder util
Applying: FS-8179
Applying: Delete unused files
Applying: FS-8287 refactor local_stream api to be more consistent and add auto complete
Applying: FS-8288 About Screen and Help link
Applying: FS-8289 #resolve [verto_communicator] make mute/unmute audio/video clickable
Applying: FS-8291 [verto_communicator] fix contributors url
Applying: FS-8195 Compatibility with Solaris 11 process privileges
Applying: FS-8243: fix typo of return from previous patch
Applying: FS-8243 8b088c26fbf4eba3daaacfaa6e29ab765f7321ba breaks perfectly working fec, adding back the missing part that actually works in most surroundings
Applying: FS-8295: mod_opus: FMTP fixes: usedtx for 8khz . useinbandfec and cbr (both 48 khz and 8 khz)
Applying: FS-8296: mod_opus: Improve the way Opus is initialized when a call comes in .
Applying: FS-8297 #resolve [Auto STUN switches IPs quickly, WebRTC video not working]
Applying: install ccache into path if it exists
Applying: test config tweaks
Applying: FS-8130
Applying: FS-8130 fix minor regression
Applying: FS-8290 #resolve [verto_communicator] automatically mark dedicated encoder if out/in bandwith isnt 'Server default'
Applying: FS-8302: fix some printing/logging because switch_opus_show_audio_bandwidth() was not returning TRUE/FALSE as expected
Applying: FS-8067 [verto_communicator] removing bind once from gravatar-src tag; using angular native bind once instead.
Applying: FS-8247 [verto_communicator] Waiting for server reconnection.
Applying: FS-8130 get poll status
Applying: FS-8130 FS-8305 fix some warnings and errors caused by dtx and/or jittery webrtc
Applying: FS-8290 [verto_communicator] adding help text on how to enable dedicated remote encoder
Applying: FS-8300 [verto_communicator] Fixing reload bug, no need to do two times now.
Applying: FS-8130 FS-8305 refactor of last patch plus suppression of scary harmless message about opus fec
Applying: FS-7924: [mod_rtmp] Modify initStream & createStream responses
Applying: FS-8315 #resolve [rtp_media_timeout not working]
Applying: FS-8316 fixed new build warning from latest clang
Applying: FS-7820 using a more appropriate function for printing diags
Applying: FS-8317 #resolve [Playing stacked video files sometimes makes the floor layer unusable]
Applying: FS-8311 [mod_voicemail] Pass session to deliver_vm
Applying: FS-8313: mod_opus: show decoder stats at end of call (how many times it did PLC or FEC)
Applying: FS-8318 #resolve [mod_av can record out of sync when video from chrome has packet loss]
Applying: uncomment code
Applying: FS-8304 #resolve [Choppy audio during calls]
Applying: FS-8321 #resolve [BEHAVIOR CHANGE Add variable media_mix_inbound_outbound_codecs to mix inbound and outbound codecs]
Applying: FS-7929 #resolve [ignore_early_media=true behaviour]
Applying: FS-8271 adding some logging, and more cautious handling of spaces in params
Applying: FS-8271 Now the default will build packages with the upstream FS package repos. This is a change in the default behavior of the debian packaging system with the justification that in 1.6 it is now required to use the FS public repo for deps because system dependancies have been removed from the FS codebase which use to be included.
Applying: FS-8233 convert unit tests frameworks to non-recursive makefiles
Applying: FS-8316 resolving the build warnings in the modules too
Applying: FS-8271 defaulting to automatically download the binary deps because without major changes to package building in cowbuilder(which is the primary supported method of building FS packages), you can't access the network to build the binary packages from the source package.
Applying: FS-8271 fixing bash typo
Applying: FS-8271 clarify help text
Applying: FS-6833 add content-type header to ack with sdp
Applying: FS-8271 If using system apt repo list, then include the supplementary ones too.
Applying: FS-8316 resolving the build warnings in one more module
Applying: FS-8179 regression setting fec_decode breaks output on stereo calls
Applying: FS-8320 #resolve [ZRTP broken in commit 06c56a037eb0b750ee41c46838a8729de9798d84]
Applying: FS-8234 #resolve
Applying: FS-8316 more clean code this way
Applying: FS-8233 Clean up formatting
Applying: bump version
Applying: FS-8194 FS-7910 FS-7937 systemd service improvements
Applying: FS-8328 'else' keyword is missing #resolve
Applying: tweak testing config #ignoreme
Applying: Enabling mod_amqp and mod_hiredis to be built by default for automation systems
Applying: FS-8329 #resolve Also fixes default configs to keep in line with a change made for Fs-7806 FS-7803
Applying: FS-8306 #resolve if the exchange doesn't exist, then create it, else
Applying: FS-8306 Now command queues can specify the queue to subscribe to. This enables very interesting use cases that would involve single job queue, and multiple consumers.
Applying: FS-8331 [verto_communicator] #resolve Do not show reconnect splash when user
Applying: FS-8319: mod_opus: fix and cleanup of switch_opus_has_fec() and switch_opus_info().
Applying: FS-8335 #resolve fix small error check that results in error message not being displayed.
Applying: FS-6833 FS-6834 found a few missing content-types in requests/resonses with sdp that were outside the norm
Applying: FS-8338 #resolve [Ringback does not work correctly on stereo channels]
Applying: FS-7834 #resolve [MOH doesn't work with inbound-bypass-media and resume-media-on-hold]
Applying: FS-6833 FS-6834 fix regression
Applying: FS-7928 FS-7618 systemd and package build improvements
Applying: FS-8287 Fix segfault from refactor
Applying: FS-8344: mod_opus: toggle FEC on the last frame which is to be packed, so that
Applying: FS-8341 [mod_distributor] fix gateway choose bug
Applying: FS-8348 Fix crash caused by trying to get channel of a null session
Applying: FS-8350 #resolve return value of SetPriorityClass() so windows build does not complain about warnings as errors on switch_core.c in set_realtime_priority()
Applying: FS-8350 quash another complaint from windows on the same issue
Applying: FS-8350: [build] fix tpl build error on windows 32 bit release build
Applying: FS-8030 [verto_communicator] added ngSanitize as a dependency, vertoFilters module and picturify filter.
Applying: FS-8030 [verto_communicator] changed chat image display behavior (break line before rendering).
Applying: FS-8298 fix libctb build
Applying: FS-8363 don't register gateways from directory, this exposes a bug where it registers over what appears to be ipv6 but doens't work correctly
Applying: FS-8338 a few regressions that were relying on this bug to function properly in stereo situations
Applying: FS-8307 #resolve [Order of codecs can cause loss of RTP stream]
Applying: FS-8366 #resolve
Applying: FS-8275 #resolve [RFC2833 DTMF broken in recent master] REGRESSION FIXED
Applying: FS-8362 #resolve Now if you install with freeswitch-all you will get the default fonts too
Applying: FS-8280: [mod_conference] remove duplicate stop-recording event and move other-recordings item over to the place its sending the event
Applying: FS-8368 #resolve [Reduce logging for audio/video sync]
Applying: FS-8369 Debian8/CentOS7 systemd installer additions
Applying: FS-8368 one more
Applying: FS-8365 [verto_communicator] fixed chat counter to increment only when the active pane is not the chat itself.
Applying: FS-8373 Improve quality of recordings when using fast encoding
Applying: FS-8372 #resolve [Wrong response to RTP/SAVPF without DTLS]
Applying: up default max jb size to 50
Applying: FS-8336 [verto_communicator] Updating mic and video overlay controls upon receiving member update from live array
Applying: FS-8375 #resolve [Add member id to conference liveArray event]
Applying: FS-8336 [verto_communicator] #resolve Using conferenceMemberID when checking if the updated member is me
Applying: FS-8377 Adding expanded support for limit_* functionality for mod_hiredis
Applying: FS-8380 Improve mod_av's handling of vw and vh core file params
Applying: FS-8378: refactor a bit to clarify code and get better debug in gdb
Applying: FS-8378: add tests for esf over loopback
Applying: FS-8378: [mod_esf] fix crash when using esf_page over loopback when transcoding
Applying: FS-8130 regression causing excessive mark bit detection in some cases
Applying: FS-8381 #resolve [Reset JB if period loss is too high]
Applying: FS-8382 #resolve [Segfault with inbound-proxy-media enabled]
Applying: FS-8115 #comment test latest patch
Applying: FS-8370 #resolve [mod_rayo] found another place in <prompt> where a message was freed after being queued for delivery. This resulted in a freed object being serialized, crashing FS.
Applying: bumping version number
Applying: FS-8384 #resolve [Locking contention in mod_conference]
Applying: FS-8389: [build] Fix msvc 2015 build warnings
Applying: FS-8391 #resolve [SDP parsing error for rtcp-fb]
Applying: FS-8281: Expose SRTP and SRTCP crypto keys as channel vars
Applying: FS-8222 [verto_communicator] updated getScreenId.js in order to detect plugin issues and attached an 'ended' event to screenshare stream in order to detect 'stop sharing' click
Applying: FS-8392: change rtpmap payload to a number in dynamic range to allow both H263 and H263+ to be offered
Applying: FS-8397: fix race condition inrementing event seq number
Applying: Target link for the plugin url, added comment explaining override $.FSRTC callback
Applying: testing config for multicanvas
Applying: FS-8398: Added event_handlers/mod_amqp to avoided modules for Ubuntu 14.04 Trusty
Applying: ESL-111 Fix esl/python/Makefile to create install directory
Applying: FS-8154 #resolve [Segmentation fault occurs while eavesdropping on video call]
Applying: FS-8369 Debian8/CentOS7 systemd installer additions
Applying: FS-8411 Replace ping_frame with video_ping_frame in a couple places that were missed
Applying: FS-8413: Segfault calling session:getVariable(nil) in lua script
Applying: FS-8308 need to double encode if urlencoding json that is already encoded
Applying: FS-8414 #resolve [Ptime unchanged on codec renegotiation]
Applying: FS-8377 Fix the handling of hiredis limit release when using an interval. The expectation for interval is to NOT decrement the limit.
Applying: FS-8415 #resolve [support early with 180 using early_use_180=true]
Applying: FS-8417 #resolve [SIP offer with a=sendonly sometimes replies with a=inactive]
Applying: FS-8404: if media engine will default to PCMU/PCMA if you don't specify any codecs
Applying: FS-8416: Regex feature in param field
Applying: FS-8400 [verto_communicator] Added Camera and microphone preview after the splash screen.
Applying: FS-8400 [verto_communicator] Removing deprecated use of stream.stop(), removing unused code and making volume meter gray so we can see it in a white background
Applying: first pass, add some funcs to conference and speed test features and fix bugs in ws.c for big payloads
Applying: WIP not shabby auto vid settings
Applying: tweaks
Applying: always change bw
Applying: FS-8293 [verto_communicator] Implemented speed test in verto communicator.
Applying: Removed unused function.
Applying: FS-8426 place freeswitch.pm into /usr/share/perl5
Applying: switch_xml_set_attr: fix inconsistent state on error paths
Applying: switch_xml_decode: avoid NUL injection
Applying: OPENZAP-240 #resolve [GSM module uses incorrect length when parsing AT responses]
Applying: commit
Applying: update
Applying: fix close file snafu
Applying: update
Applying: update
Applying: FS-8427 Incompatible type for %ld in prinrtf
Applying: FS-8369 Fixes
Applying: FS-8432 fixes timestamp type resolution, adds new header of type uint64 to carry microsecond resolution timestamp
Applying: FS-8530 add codecs/mod_sangoma_codec to avoid_mods in debian/bootstrap.sh
Applying: Revert "FS-8530 add codecs/mod_sangoma_codec to avoid_mods in debian/bootstrap.sh"
Applying: FS-8293 [verto_communicator] - Showing speed in the menu bar if autoBand is true, adding option to test speed before making a call, enabling dedEnc if inboundBandwidth is below dedEncWatermark (3072 by default).
Applying: FS-8534: calculate RTT average (RTCP SR)
Applying: update new pass
Applying: add colors to good and bad
Applying: OPENZAP-238: [freetdm] Several core and gsm improvements
Using index info to reconstruct a base tree...
M       libs/freetdm/src/include/private/ftdm_core.h
Falling back to patching base and 3-way merge...
Auto-merging libs/freetdm/src/include/private/ftdm_core.h
Applying: OPENZAP-238: [freetdm] Fix gsm forwarding initialization
Applying: OPENZAP-238: [freetdm] Fix state transition on hangup after a raw call is placed
Applying: OPENZAP-238: [freetdm] Confirm release on hangup of raw GSM call
Applying: OPENZAP-238: [freetdm] Enable GSM immediate forwarding logic
Applying: OPENZAP-238: [freetdm] Add new GSM parameter startup-command
Applying: OPENZAP-238: [freetdm] Fix stop sequence to properly shutdown the gsm span using libwat
Applying: OPENZAP-238: [freetdm] Fix gsm signaling status reporting
Applying: OPENZAP-238: [freetdm] Fix gsm caller id and dnis information
Applying: FS-8293 update position
Applying: FS-8425 #resolve [DTMF sometimes missed on PSTN call]
Applying: FS-8536 #resolve [Send Keyframe when getting SIP INFO with picture_fast_update]
Applying: FS-8537: Passing nil to various lua functions causes segfault
Applying: FS-8543 #resolve [Improve mute handling on conference and WebRTC]
Applying: FS-8545 #resolve [Improve controls for screen share]
Applying: remove DEBUG
Applying: FS-8546 #resolve [Make original video demo back-compat with livearray-json-status]
Applying: FS-8545 add missing code to deal with screen share part
Applying: FS-8527 [mod_conference] Do not send the video of last_video_floor_holder to video_floor_holder if already related one video to it.
Applying: FS-8333
Applying: FS-8549 [mod_http_cache] add support for AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY environment variables in S3 profiles
Applying: FS-8529 #resolve [vid-floor and conference-flags set as video-muxing-personal-canvas ]
Applying: FS-8550 [verto_communicator] - Now setting testSpeedJoin to false when 'auto' is disabled and changed instruction for closing modal in resetSettings()
Applying: FS-8542 [verto_communicator] - fixed the tooltips of video controls...
Applying: FS-8401 [verto_communicator] - Added Speaker selection in settings modal and video page.
Applying: FS-8547 #resolve [Add error log into stats to log when quality impacting events begin and end]
Applying: FS-8053 #resolve [When WebRTC's SDP contains a=sendonly for video, the client will still receive the video stream]
Applying: FS-8053 typo
Applying: FS-8053 typo2
Applying: FS-8553 [config] include verto_contact into the dial-string in the samples
Applying: FS-8053 amendment
Applying: FS-8545 read lock regression
Applying: FS-8545 do not allow video floor on a member with a reservation id set
Applying: FS-8401 refactor the sinkid function into verto lib
Applying: FS-8556 #resolve [Screen shares are not recoverable so do not try]
Applying: FS-8529 fix a few regressions
Applying: FS-8293 fix some regressions where speed test caused excessive downlink bandwidth
Applying: FS-8559: mod_shout should have "mpga" in it's list of supported extensions
Applying: tweak
Applying: tweaks %ignore
Applying: tweak ignore
Applying: FS-8160 Additional vulnerability in json parsing malformed utf encoded chars discovered by Brian Martin - Tenable Security Response CVE-2015-7392
Applying: bump revision
Applying: FS-8560 [mod_http_cache] add new parameter, connect-timeout
Applying: FS-8543 make sure if the mute image is taken from the camera that it does not dissappear
Applying: FS-8554 #resolve [VC Moderator button quit working after moderator screen shares]
Applying: Revert "FS-8369 Fixes"
Applying: Revert "FS-8369 Debian8/CentOS7 systemd installer additions"
Applying: Revert "FS-8369 Debian8/CentOS7 systemd installer additions"
Applying: tweak to freeswitch.spec for building RPMs on centos7
Applying: Revert "FS-8369 Fixes"
Using index info to reconstruct a base tree...
M       build/Makefile.am
A       build/startup/freeswitch.service.in
A       build/startup/install_systemd.sh.in
M       configure.ac
Falling back to patching base and 3-way merge...
Auto-merging configure.ac
CONFLICT (content): Merge conflict in configure.ac
CONFLICT (modify/delete): build/startup/install_systemd.sh.in deleted in HEAD and modified in Revert "FS-8369 Fixes". Version Revert "FS-8369 Fixes" of build/startup/install_systemd.sh.in left in tree.
CONFLICT (modify/delete): build/startup/freeswitch.service.in deleted in HEAD and modified in Revert "FS-8369 Fixes". Version Revert "FS-8369 Fixes" of build/startup/freeswitch.service.in left in tree.
Auto-merging build/Makefile.am
CONFLICT (content): Merge conflict in build/Makefile.am
error: Failed to merge in the changes.
hint: Use 'git am --show-current-patch' to see the failed patch
Patch failed at 0403 Revert "FS-8369 Fixes"
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
rebase in progress; onto 5933f696bf
You are currently rebasing.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add/rm <file>..." as appropriate to mark resolution)

        both modified:   build/Makefile.am
        deleted by us:   build/startup/freeswitch.service.in
        deleted by us:   build/startup/install_systemd.sh.in
        both modified:   configure.ac

no changes added to commit (use "git add" and/or "git commit -a")

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rm build/startup/freeswitch.service.in build/startup/install_systemd.sh.in
build/startup/freeswitch.service.in: needs merge
build/startup/install_systemd.sh.in: needs merge
rm 'build/startup/freeswitch.service.in'
rm 'build/startup/install_systemd.sh.in'

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
rebase in progress; onto 5933f696bf
You are currently rebasing.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

        both modified:   build/Makefile.am
        both modified:   configure.ac

no changes added to commit (use "git add" and/or "git commit -a")

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git add build/Makefile.am configure.ac

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
Applying: Revert "FS-8369 Fixes"
No changes - did you forget to use 'git add'?
If there is nothing left to stage, chances are that something else
already introduced the same changes; you might want to skip this patch.
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
rebase in progress; onto 5933f696bf
You are currently rebasing.
  (all conflicts fixed: run "git rebase --continue")

nothing to commit, working tree clean

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
Applying: Revert "FS-8369 Fixes"
No changes - did you forget to use 'git add'?
If there is nothing left to stage, chances are that something else
already introduced the same changes; you might want to skip this patch.
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --continue
Applying: Revert "FS-8369 Fixes"
No changes - did you forget to use 'git add'?
If there is nothing left to stage, chances are that something else
already introduced the same changes; you might want to skip this patch.
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".

A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
rebase in progress; onto 5933f696bf
You are currently rebasing.
  (all conflicts fixed: run "git rebase --continue")

nothing to commit, working tree clean

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --skip
Applying: Revert "FS-8369 Debian8/CentOS7 systemd installer additions"
.git/rebase-apply/patch:253: trailing whitespace.
echo
warning: 1 line adds whitespace errors.
Using index info to reconstruct a base tree...
M       build/Makefile.am
A       build/startup/freeswitch.default
A       build/startup/freeswitch.service.in
A       build/startup/freeswitch.tmpfile
A       build/startup/install_systemd.sh.in
M       configure.ac
Falling back to patching base and 3-way merge...
CONFLICT (rename/delete): build/startup/install_systemd.sh.in deleted in HEAD and renamed to init/install_systemd.sh.in in Revert "FS-8369 Debian8/CentOS7 systemd installer additions". Version Revert "FS-8369 Debian8/CentOS7 systemd installer additions" of init/install_systemd.sh.in left in tree.
CONFLICT (rename/delete): build/startup/freeswitch.service.in deleted in HEAD and renamed to init/freeswitch.service.in in Revert "FS-8369 Debian8/CentOS7 systemd installer additions". Version Revert "FS-8369 Debian8/CentOS7 systemd installer additions" of init/freeswitch.service.in left in tree.
CONFLICT (rename/rename): Rename "build/startup/freeswitch.default"->"debian/freeswitch-sysvinit.freeswitch.default" in branch "HEAD" rename "build/startup/freeswitch.default"->"init/freeswitch.default" in "Revert "FS-8369 Debian8/CentOS7 systemd installer additions""
CONFLICT (rename/rename): Rename "build/startup/freeswitch.tmpfile"->"debian/freeswitch-systemd.freeswitch.tmpfile" in branch "HEAD" rename "build/startup/freeswitch.tmpfile"->"init/freeswitch.tmpfile" in "Revert "FS-8369 Debian8/CentOS7 systemd installer additions""
Auto-merging configure.ac
CONFLICT (content): Merge conflict in configure.ac
Auto-merging build/Makefile.am
CONFLICT (content): Merge conflict in build/Makefile.am
error: Failed to merge in the changes.
hint: Use 'git am --show-current-patch' to see the failed patch
Patch failed at 0404 Revert "FS-8369 Debian8/CentOS7 systemd installer additions"
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
rebase in progress; onto 5933f696bf
You are currently rebasing.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add/rm <file>..." as appropriate to mark resolution)

        both modified:   build/Makefile.am
        both deleted:    build/startup/freeswitch.default
        both deleted:    build/startup/freeswitch.tmpfile
        both modified:   configure.ac
        added by us:     debian/freeswitch-systemd.freeswitch.tmpfile
        added by us:     debian/freeswitch-sysvinit.freeswitch.default
        added by them:   init/freeswitch.default
        added by them:   init/freeswitch.service.in
        added by them:   init/freeswitch.tmpfile
        added by them:   init/install_systemd.sh.in

no changes added to commit (use "git add" and/or "git commit -a")

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rm build/startup/freeswitch.default  build/startup/freeswitch.tmpfile
build/startup/freeswitch.default: needs merge
build/startup/freeswitch.tmpfile: needs merge
rm 'build/startup/freeswitch.default'
rm 'build/startup/freeswitch.tmpfile'

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --skip
Applying: Revert "FS-8369 Debian8/CentOS7 systemd installer additions"
Using index info to reconstruct a base tree...
M       build/Makefile.am
M       configure.ac
A       init/freeswitch.default
A       init/freeswitch.service.in
A       init/freeswitch.tmpfile
A       init/install_systemd.sh.in
Falling back to patching base and 3-way merge...
CONFLICT (rename/delete): init/freeswitch.default deleted in Revert "FS-8369 Debian8/CentOS7 systemd installer additions" and renamed to debian/freeswitch-sysvinit.freeswitch.default in HEAD. Version HEAD of debian/freeswitch-sysvinit.freeswitch.default left in tree.
CONFLICT (rename/delete): init/freeswitch.tmpfile deleted in Revert "FS-8369 Debian8/CentOS7 systemd installer additions" and renamed to debian/freeswitch-systemd.freeswitch.tmpfile in HEAD. Version HEAD of debian/freeswitch-systemd.freeswitch.tmpfile left in tree.
error: Failed to merge in the changes.
hint: Use 'git am --show-current-patch' to see the failed patch
Patch failed at 0405 Revert "FS-8369 Debian8/CentOS7 systemd installer additions"
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git rebase --skip
Applying: tweak to freeswitch.spec for building RPMs on centos7
Using index info to reconstruct a base tree...
M       freeswitch.spec
Falling back to patching base and 3-way merge...
No changes -- Patch already applied.
Applying: FS-8293 fix some miscues on this patch
Applying: simple compile fixes for windows
Applying: FS-8565 update initial rtp from address when auto changing port so we can avoid false later comparisons
Applying: Allow building using system OpenSSL without EC support
Applying: Fix undefined symbol conference_cdr_test_mflag
Applying: Added AC_FUNC_MEMMOVE test to spandsp
Applying: FS-8566 #resolve [Call fails when put on hold in bypass media mode with inbound late neg set to false]
Applying: FS-8573 #resolve [unidirectional audio after resuming from hold in bypass media mode]
Applying: FS-8575 #resolve [No DTMF passed from a to b during rfc 2833 events.]
Applying: FS-8293 make sanity level based on 1080p
Applying: FS-8293 make sanity level based on quality 2 and also in conference
Applying: FS-8293 make conference video quality configurable with video-quality param NEEDS DOC
Applying: FS-8152 package the image directories too
Applying: FS-8576 resolving issue where fonts and images were installed in all of the conf packages.
Applying: FS-8578: [mod_verto] fix build error for missing __bswap_64 on osx
Applying: FS-8582 #resolve make sure the URL being passed here is not null
Applying: FS-8364: mod_codec2 : improvements, add extra modes: 3200,1400,1200. add config file.
Applying: FS-7800 [verto_communicator] - Added an option in user's hover buttons to open another canvas when canvasCount is higher than 1
Applying: FS-8574 #resolve [hangup a call but still in show calls (uuid_kill result -ERR No such channel!)]
Applying: FS-8433: allow hangup cause to be set inside redirect data
Applying: FS-8264 #resolve [Add all the reservation IDs in the return of "list-videoLayouts" command]
Applying: FS-8264 [verto_communicator] - Adapted the layout select to new response, added a separated menu in members list to set its resevartion id
Applying: FS-7800 [verto_communicator] - Added Canvas controls and now opening popup with original (master)  dimensions
Using index info to reconstruct a base tree...
M       html5/verto/verto_communicator/src/partials/chat.html
M       html5/verto/verto_communicator/src/vertoControllers/controllers/ChatController.js
M       html5/verto/verto_communicator/src/vertoControllers/controllers/InCallController.js
M       html5/verto/verto_communicator/src/vertoService/services/vertoService.js
Falling back to patching base and 3-way merge...
Auto-merging html5/verto/verto_communicator/src/vertoService/services/vertoService.js
Auto-merging html5/verto/verto_communicator/src/vertoControllers/controllers/InCallController.js
Auto-merging html5/verto/verto_communicator/src/vertoControllers/controllers/ChatController.js
Auto-merging html5/verto/verto_communicator/src/partials/chat.html
Applying: FS-8585: [mod_commands] group_call: expand {} and <> to [] for each dial string
Applying: FS-8589 #resolve [Using conference playback with full-screen=true is not working correctly]
Applying: FS-8590 #resolve [Verto Communicator sends malformed vid-res-id command when changing layouts]
Applying: FS-8529 more improvment on same goal
Applying: FS-8595 #resolve [Improve auto bitrate in personal canvas mode and do not let auto bitrate exceed native picture size]
Applying: FS-8595 typo
Applying: FS-8588 #resolve [Unreliable digit collection]
Applying: FS-8588 fix deadlock in error condition
Applying: FS-8588 remove debug
Applying: FS-8599 #resolve [Workaround for Mozilla is no longer needed for video size]
Applying: FS-8354 #resolve [G722 audio issues with mod_conference after fab435479ada61f2f9d726bad53ec31d002acd2f]
Applying: FS-8590 treat no res-id the same as clear
Applying: FS-8602 #resolve [conference does not auto-generate layouts properly when callers with no camera are present]
Applying: FS-8612 #resolve [rare ivr originated calls crash due to read codec leak]
Applying: Tweaks for platforms which require string.h for memxxx functions.
Applying: FS-8614 #resolve Add debian developers install script and update README.md to reference it
Applying: FS-8615 #resolve [Crash when quickly changing layouts and setting reservation ids.]
Applying: FS-8617 #resolve [Add gain and vol seperate to verto]
Applying: FS-8619 [mod_rayo] reply with conflict stanza error if bind is attempted with duplicate JID.  Improve error handling when 'ready' callback fails.
Applying: misc config update for available modules. #noWIR
Applying: FS-8625 #resolve [Segmentation fault: 11]
Applying: FS-8603 [verto_communicator] - Added Device validation
Applying: FS-8621
Applying: FS-8616 [verto_communicator] - A new menu for moderator, added Gain buttons and removed the 3-dot-button, moving its behavior to member-name div
Applying: FS-8631 [update regex to allow DSN to match rest of FS code]
Applying: FS-8632 #resolve [Add origination_audio_mode originate variable]
Applying: FS-8640 #resolve Dont clear conference member reservation id on members that dont have a reservation ID
Applying: FS-8293 add quality level 0 to conference (default is 1) and fix some logic in auto bw
Applying: FS-8633 #resolve [ first verto to join a conference does not get "conference-livearray-join" event]
Applying: FS-8641 hard code to 15 for now until this is complete
Applying: FS-8641 need to set min and max for less than 30 but if you set min and max to 30 and it doesn't support it it will fail, (maybe design these FPS params a little better webrtc politicians)
Applying: FS-8642 #resolve [CF_VIDEO_READY set on non-video calls]
Applying: FS-8641 [verto-communicator] - Added Frame Rate setting
Applying: FS-8643: fix mod_sofia memory leaks
Applying: revert commit from PR 585
Applying: Revert "explore use of nuget for wix build dependency"
Applying: FS-8650 #resolve [Add switch_must_malloc/switch_must_realloc/switch_must_strdup and use it in switch_xml.c]
Applying: FS-8655 #resolve Update the windows build for MSVS2015 and WiX 3.10.1 to build an MSI
Applying: FS-9658: [mod_verto] windows fixes for mod_verto
Applying: FS-8642 addtl patch
Applying: FS-8658: [mod_verto] windows fixes for mod_verto
Using index info to reconstruct a base tree...
M       src/mod/applications/mod_conference/conference_video.c
M       src/switch_channel.c
Falling back to patching base and 3-way merge...
No changes -- Patch already applied.
Applying: FS-8648: incorrect url's in yum repos
Applying: FS-8629 #resolve [Add new param video-mute-exit-canvas to conference cflags]
Applying: FS-8663 #resolve [add vid-personal command]
Applying: FS-8662 [mod_http_cache] don't block http_tryget while another thread is fetching the URL
Applying: FS-8663 a little more
Applying: FS-8664 #resolve add fs_cli.exe to msi so its installed.
Applying: FS-8654 [verto_communicator] - Added event emission on hangup
Applying: FS-8641 #resolve [Make frame rate configurable]
Applying: FS-8595 matching input res does not work well when chrome auto decides to shrink it too small so never mind this part
Applying: bugfix: prevented endless loop in sendmsg
Applying: FS-8668: allow channel variable prefix-a-leg to override global setting in mod_xml_cdr
Applying: FS-8595 contd
Applying: FS-8595 contd
Applying: FS-8595 contd
Applying: FS-8595 contd
Applying: FS-8179 bring this back to default its fixed in chrome
Applying: Use mtmalloc on Solaris SPARC if available
Applying: FS-8636 [verto_communicator] - Fix URI mismatch before angular bootstraps
Applying: FS-8595 contd
Applying: FS-8595 contd
Applying: FS-8675 [verto_communicator] - Fixed angular-bootstrap version at resolutions
Applying: FS-8563 [verto_communicator] - Added callback to setAudioPlaybackDevice
Applying: FS-8591 [verto_communicator] - Changed ng-keyup to ng-keydown
Applying: FS-8591 [verto_communicator] restoring chat send button behavior
Applying: FS-8600 [verto_communicator] - Added button at top menu for new window
Applying: FS-8673 #resolve [core dump on playback after "Decode Codec is not initialized!" log message]
Applying: FS-8676 [mod_unimrcp] fix crash when an invalid voice-gender TTS param is attempted to be set
Applying: add parsing for userLocation header
Applying: add parsing for userLocation header
Applying: FS-8329 update the example configs, and fix whitespace
Applying: FS-8692 Added functionality to mod_amqp command listener to be able to send the api response back as an event to an exchange defined in the api message headers
Applying: FS-8694 list_users performance issue update
Applying: Add missing ENABLE_SRTP ifdef to allow building without SRTP
Applying: FS-8683: [mod_conference] fix seg originating non video call into mcu video conference
Applying: Fix typo in freeswitch-doc package description
Applying: FS-8673 #resolve [core dump on playback after "Decode Codec is not initialized!" log message]
Applying: FS-8673
Applying: FS-8424 fix for default rounding values
Applying: FS-8679: [mod_sofia] no missed call if call is anwered by someone else
Applying: FS-8704: Add min-members, wait-min-members-timeout, wait-mod-timeout, wait-min-members-timeout-message, wait-mod-timeout-message, endconf-mod-exit-message, and endconf-message parameters and functionality to mod_conference.
Applying: FS-8706: Fix a segfault where no response status was set previously, and try to prevent future ones by setting default response status of 500.
Applying: FS-8708 [mod_rayo] fix example configuration to map to correct DETECTED_TONE event from spandsp_start_tone_detect
Applying: FS-8677 #resolve [Crash (possible memory corruption) after codec change]
Applying: FS-8711 #resolve [fix a couple of possible memory leaks in mod_skinny packet reading code]
Applying: FS-8711 #resolve [fix a couple of possible memory leaks in mod_skinny packet reading code]
Using index info to reconstruct a base tree...
M       src/mod/endpoints/mod_skinny/mod_skinny.c
M       src/mod/endpoints/mod_skinny/skinny_protocol.c
Falling back to patching base and 3-way merge...
No changes -- Patch already applied.
Applying: mod_skinny: control flow issue reported by coverity CID 1294487
Applying: mod_skinny: silence CID 1320795 by rearranging mutex aquisition, negligible impact
Applying: mod_skinny: remove nested redundant mutex that could cause a hang
Applying: FS-8713 #resolve avoid read exceeding buffer
Applying: Revert "FS-8713 #resolve avoid read exceeding buffer"
Applying: FS-8713 avoid write exceeding buffer
Applying: FS-8716 #resolve [recording offset is delayed few seconds for rtmp stream]
Applying: FS-8716 typo
Applying: FS-8716 actually fix typo
Applying: FS-8715: [mod_sofia] make the oubound_proxy on the profile consistent with how we do the same thing on the gateway
Applying: Adding a file extension to the package build logs
Applying: FS-8716
Applying: version bump
Applying: FS-8721: avoid memory leak when removing thread-locked bugs
Applying: FS-8720: [core] allow passing in blank/null to session::hangup to mean NORMAL_CLEARING hangup cause instead of segfault
Applying: FS-8719: [mod_conference] fix seg when building without video support, but specifying video_mute_png variable for a conference member
Applying: Revert "FS-8720: [core] allow passing in blank/null to session::hangup to mean NORMAL_CLEARING hangup cause instead of segfault"
Applying: FS-8720 #resolve [Segmentation Fault when switch_channel_str2cause is called]
Applying: FS-8728 Adding logging profile and functionality
Applying: FS-8731 #resolve [Crash when leg-b invite video in voice call] #comment please update and retest
Applying: FS-8728 default configs
Applying: FS-8734 #resolve [Video JitterBuffer Cleanup]
Applying: FS-8735 display update support for Panasonic devices
Applying: FS-8713 #resolve [crash on bad video rtp stream] #comment Pushed a patch to make the sizes match.  This was the original intention since we want to preserve the packet as-is while in the jb
Applying: FS-8736 #resolve [Missing MEMMOVE macro in spandsp autoconf]
Applying: FS-8721 #resolve [Bug removal at call end causes eavesdrop memory leak] #comment This should do it, bug_remove_all is called after destroy where it's more than safe to kill bugs indiscriminately
Applying: FS-8673 #resolve [core dump on playback after "Decode Codec is not initialized!" log message]
Applying: FS-8721 keep track of last on continue cases
Applying: and debug code vpx params
Applying: FS-7776 build kazoo module
Applying: FS-8721 last should only be set on the ones we skip that will not be destroyed
Applying: FS-8756: [mod_say_nl] Improve dutch localisation
Applying: FS-8758: scroll properly numbers in call history div
Applying: FS-8737: [mod_kazoo] Add require vars to default filter
Applying: FS-8763 [mod_sofia] set is_auth on switch_ivr_set_user result
Applying: FS-8759 #resolve [segfault on 1.7] #comment please update to master
Applying: FS-8111 [mod_sofia] 'sofia' API command auto-complete cleanup
Applying: FS-8685: multiple member arguments for conference relate api command
Applying: update my email address
Applying: Fixed the application of the T.30 T1 timeout in faxing.
Applying: FS-8688 #comment add more detailed logs and tweak some params for the comming libvpx 1.5.0
Applying: FS_8775 Add function SST_SHORT_DATE_TIME to mod_say_de.c and some language tweaks
Applying: FS-8770 #resolve [media_bug_answer_req=true results in a file being created for unanswered calls]
Applying: FS-8656: Fix switch_event_channel_broadcast identifier redeclared
Applying: FS-8777 #resolve FreeBSD: mod_redis/credis.c missing netinet/in.h include
Applying: Applied the Sangoma patch to FS version 1.2 in order to port to FS v1.6+
Applying: FS-8786 #resolve [rtp-timeout-sec on profile triggers when receiving T.38 UDPTL frames]
Applying: FS-8787 #resolve [fs_cli prompt modifications]
Applying: FS-8788 Enabling stacksize limits for Debian packaging
Applying: FS-8595 lower the q size by 1
Applying: FS-8782: [mod_verto] fix build error for missing __bswap_64 on Solaris
Applying: Allow Solaris privileges to work on both Solaris and derivatives
Applying: FS-8792: apply video resolution on combobox video quality changes
Applying: FS-8796 #resolve [issues with verto mcast]
Applying: FS-8798 #resolve [Typo in verto FSRTC lib]
Applying: FS-8776 #resolve Support GNU make parallel builds
Applying: FS-8799 [mod_callcenter] Add JSON API interface for mod_callcenter
Applying: FS-8768 - [mod_callcenter] Releasing db handle after reserving agent
Applying: FS-8778 #resolve FreeBSD: src/mod/endpoints/mod_verto/ws.h define __bswap_64
Applying: FS-8802 #resolve [RTP stops sending audio when sent timestamp rolls over]
Applying: Clean up whitespace, and remove surplus log line at crit level. #NOWIR
Applying: FS-8789 fix warning thats printed when it shouldn't be
Applying: FS-6833 #resolve [Allow Freeswitch to initiate Late offer calls.] #comment Regression from addition of custom variables
Applying: FS-8789: remove ability to swap to personal canvas while recording and prevent recording while personal canvas is on.
Applying: FS-8805: sanity check on array
Applying: FS-8805: sanity check on array
Applying: FS-8726 #resolve [Spurious case of thread stuck and saturating CPU]
Applying: FS-6833 ammendium
Applying: refactor: fix comments
Applying: FS-8779: [mod_shout] properly detect lame/lame.h
Applying: FS-6833
Applying: FS-6833
Applying: refactor: fix comments
Applying: refactor: fix comments
Applying: FS-6544
Applying: FS-6833
Applying: refactor: fix comments
Applying: FS-6544 fix return type
Applying: FS-8822 - [mod_callcenter] Realtime counter for  calls in a queue
Applying: FS-8812 Respect in_thread_only parameter in switch_channel_check_signal()
Applying: FS-8808 #resolve fixed ^D in fs_cli with editline to delete char under cursor, not just backspace
Applying: FS-8809 fix MAP_POPULATE undeclared
Applying: FS-8816 #resolve [switch_hashtable_insert_destructor() returns 0/-1 but switch_core_hash_insert_destructor never checks this]
Applying: FS-8814: fix build on older libedit
Applying: FS-8818 #resolve refactor X-PRE include to not toss error if there are not files that match the include
Applying: FS-8821 #resolve [Check for status of executed operation]
Applying: swigall
Applying: FS-8830 SDP line separator fix for SDP generated by the core
Applying: FAX now applies the T1 timer properly after mid call returns to phase B.
Applying: FS-8838 [mod_rayo] improve logging and error handling when executing API on output component.  Attempt to make output component stop a little more robust.
Applying: FS-8838 [mod_rayo] fix syntax error in previous commit
Applying: FS-8838 [mod_rayo] Do a better job of detecting when output component completed because of hangup and preventing operations on output component when call has ended.
Applying: FS-8839 - [verto_communicator] Fix screenshare when caller is transferred to another conference.
Applying: FS-8806 Change group_confirm_cancel_timeout to apply only to the legs that answer the call.
Applying: FS-8811 #resolve [FS 1.7 crashes intermittently]
Applying: small tweak to debian package build script
Applying: FS-8688 #resolve [Implement VP9 draft uberti payload 01 and libVPX 1.5]
Applying: FS-7132
Applying: FS-8821
Applying: FS-8847 #resolve [Silence Error on shutdown of video call]
Applying: FS-8852 change stop condition in for loop
Applying: FS-8854 initialize circular buffer
Applying: FS-8856 - [mod_callcenter] Inserting member as a new one when restoring action fails because our agent_dispatch_thread removed the members just before we tried to update him.
Applying: FS-8842 #comment please try this patch on latest master
Applying: test script
Applying: add libvpx 89cc6825 (matching current chrome canary) from repo https://chromium.googlesource.com/webm/libvpx
Applying: import libyuv at hash 38d37a5 from https://chromium.googlesource.com/libyuv/libyuv/
Applying: add pic
Applying: update verto-min
Applying: FS-8867: create conversion function stubs in the core so modules do not need to use libyuv directly
Applying: FS-8867: build using in tree libyuv to match required version and not impact system ones that are never sufficient version
Applying: FS-8862 auto adjust on passthru
Applying: FS-8867: build using in tree libvpx, vpx no longer optional and does not use system libvpx due to issues with having to update it frequently conflicting with system libraries, now we link to the static in tree version instead.  Also, mod_vpx is now a core module instead of a loadable module, so mod_vpx.so will no longer be built
Applying: FS-8867 Adding changes to the debian package building
Applying: FS-8867: do hidden visibility on vpx as we don't want symbols leaking out of libfreeswitch, and we get relocation error on a vpx symbol anyways
Applying: FS-8871: Fixed encoding "&" and "<" symbols in vanilla config
Applying: FS-8876 #resolve [Bind video threads to CPU alternating]
Applying: FS-8877 #resolve [Chrome Canary removed some audio mandatory constraints that break Verto]
Applying: FS-8878: mod_amr: make AMR NB transcode in octet-align mode (when compiled with HAVE_AMR)
Applying: FS-8864 #resolve [Improve video file playback]
Applying: FS-8864
Applying: FS-8864
Applying: FS-8879 #resolve [SIP UPDATE and attended transfer]
Applying: FS-8864
Applying: FS-8884: add --disable-libyuv and --disable-libvpx configure args to disable building these libraries
Applying: FS-8876 make function public and use it in conference also
Applying: FS-8864 set video ready on first push to avoid catch 22 on some video files
Applying: FS-8891 #resolve [T38 fax fails between 2 freeswitch boxes, with high cpu usage (comparison overflow?)]
Applying: swigall
Applying: FS-8354 #resolve [G722 audio issues with mod_conference after fab435479ada61f2f9d726bad53ec31d002acd2f]
Applying: FS-8811 #comment please test
Applying: FS-8851 #resolve [Codec for recording is negotiated before call answered]
Applying: Removing libyuv as a required dep since it's moved to core.
Applying: FS-8903 #resolve [Add logo img to local_stream]
Applying: FS-8811 #resolve [FS 1.7 crashes intermittently]
Applying: FS-8904 #resolve [Fix mem leak in img_write_text]
Applying: FS-8905 doing -1 here prevents the crash, its something to do with the last row in the height
Applying: FS-8905
Applying: FS-8811 fix divide by 0
Applying: FS-8905 #comment fix possible out of boundary access, please test
Applying: FS-8905 #resolve [Heap overflow in img patch]
Applying: Fix memory leaks
Applying: FS-8878 fix compiling without the library installed
Applying: FS-8908 - [verto_communicator] Link to preview camera and mic under settings
Applying: FS-8864 fix regression to recording
Applying: FS-8909 #resolve [Add feature to play background video while recording inbound video]
Applying: FS-8868 #resolve [recording app to respect bandwidth set in SDP]
Applying: FS-8910: Properly negotiate SDES when receiving SDP with a=crypto:0
Applying: FS-8911: fix typo in conference_member
Applying: FS-8914 #resolve [recording mp4 cuts off the end in some cases]
Applying: FS-8914
Applying: tweak
Applying: revert
Applying: FS-8914
Applying: FS-8914
Applying: FS-8916 #resolve
Applying: FS-8916 comment out dead code from the last bug fix, add to TODO
Applying: borrow code from ffmpeg to fix log_packet function for debuging
Applying: FS-8909 add record_indication variable/param with path to beep sound etc
Applying: FS-8761 #resolve [Memory leak in FreeSWITCH]
Applying: FS-7389 tweak the freeswitch users homedir to be correct in the specfile
Applying: sigh, dss keys are depricated
Applying: FS-8909 FS-8914
Applying: FS-8909 FS-8914 refactoring
Applying: swigall
Applying: FS-8898 log setVariable at debug, so you can tell what variables are being set with ease from scripts
Applying: FS-8921 #resolve [Tweak banner code in mod conference]
Applying: FS-8836 #comment WIP codec should working now
Applying: OPENZAP-241: set always STATE_HANGUP_COMPLETE
Applying: FS-8915 event header name shortened.
Applying: FS-8909 FS-8914
Applying: FS-8752 #resolve [When recording a conference, the first 2 seconds are pixelated]
Applying: FS-8836
Applying: FS-8853 enable change of resolution of fast arc cos table
Applying: FS-8810 fix crash on FS startup
Using index info to reconstruct a base tree...
M       src/mod/applications/mod_avmd/fast_acosf.c
Falling back to patching base and 3-way merge...
Auto-merging src/mod/applications/mod_avmd/fast_acosf.c
Applying: FS-7286 add file_interface support to mod_memcache
Applying: FS-8932 - Add in process loading via LoadEmbeddedPlugins
Applying: FS-8933 WIP initial commit of bash/dialog based installer for debian and raspbian \ debian support working, raspbian needs some work
Applying: FS-8933 WIP make it not prompt for apt-get install
Applying: version bump
Applying: FS-8933 WIP few more tweaks
Applying: FS-8936 - Added swig typemap for "const char **" for fix invocation problems, reswig
Applying: FS-8933 WIP a few more edits to make Raspbian work
Applying: FS-8938 #resolve [Clear res id when setting same res id to another member]
Applying: FS-8933 #resolve Basic FreeSWITCH from source installer that works on Raspbian and Debian. Also installs VertoCommunicator and LetsEncrypt SSL Certs. LetsEncrypt requires the machine to have a public IP and DNS for the FDQN functioning properly in public DNS
Applying: rename the raspbian installer
Applying: FS-8942: pass compiler to libvpx configure
Applying: FS-8943 Fixed misspellings in two comments
Applying: FS-8870: add human-readable call quality statistics logs on call hangup
Applying: FS-8950 fix a few memory leaks in mod_skinny
Applying: FS-8946: [mod_xml_cdr] fix segfault on call after loading with no config file or event bind failure causing module load failure
Applying: FS-8937: [mod_easyroute] handle segfault when using bad customer query or on query error
Applying: FS-8951 #resolve [Video lockup in conference due to race condition]
Applying: FS-8750 implement file_seek for video files
Applying: FS-8754 add ability to read high quality PDF
Applying: FS-8748 track pdf total pages and current page
Applying: FS-8748 FS-8751 check current play_status
Applying: FS-8751 [Conference Play Video Total Time and Current Time]
Applying: FS-8855 fix calc of variance of tone's freq estimator
Applying: FS-8953 [core] white space clean up.
Applying: FS-8948 #resolve Handle non-existent config in Debian postinst
Applying: FS-8952: fix unreachable code and simplify conditions
Applying: FS-8928: flag a bidning error when using EventConsumer::bind with invalid event name instead of blindly using custom
Applying: FS-8168: use copy image functions from libyuv instead of our home rolled versions as the libyuv versions have optimizations
Applying: FS-8841 #resolve [Debian - FS Hang ]
Applying: FS-8663 add saftey checks for this command
Applying: FS-8406 #comment add options to scale down cavas size, fps and bandwidth
Applying: FS-8406 #resolve #comment improvement to drop video packets on slow rtmp link
Applying: fix typo from 8e7cfce5641fce466b0d41df7f5aa336e821ee6c
Applying: FS-8663 cont
Applying: FS-7915 parse and store multiple path fields
Applying: FS-8945 - [verto_communicator]  don't show preview settings during video call
Applying: FS-8942: pass along CFLAGS, CXXFLAGS, and LDFLAGS to vpx build too, fixes solaris 64bit build
Applying: FS-8957 #resolve [Video image sometimes blips on personal canvas mode when 1 participant is watching voh]
Applying: FS-7800 should be able to call extra screens with same extension as the original and place the param conferenceCanvasID with the desired canvas id into the call params in the same place bandwidth prefs are added
Applying: FS-7800 fix some stuff in multi-canvas
Applying: FS-7800 add canvas id arg to setVideoLayout
Applying: FS-8960 Set buffer position to beginning on reset
Applying: Small adjustment to mod_sangoma_codec
Applying: FS-8961 Increase robustness of estimation
Using index info to reconstruct a base tree...
M       src/mod/applications/mod_avmd/mod_avmd.c
Falling back to patching base and 3-way merge...
Auto-merging src/mod/applications/mod_avmd/mod_avmd.c
Applying: Reset the whole transcoding session memory on destroy
Applying: FS-8963 - [verto_communicator]  fix label status call
Applying: FS-8964 #resolve [Make it possible to disable picture_fast_update INFO requests]
Applying: FS-8959 refactor improve memory processing
Applying: FS-8966 - [verto_communicator] Ability to cancel dialing while doing speed test
Applying: FS-8970 - [verto_communicator] Setting 'none' for useSpeak/useMic/useCamera when calling a secondary canvas
Applying: Settings modal rework -> panel slider at top
Applying: FS-8914 feed NULL to flush encoder at the end of recording, this fixes possible infinite loop
Applying: FS-8973 #resolve
Applying: FS-7800 disable video floor changes on multi canvas
Applying: FS-8975 #resolve [DTMF variables not functioning]
Applying:  FS-8959: [mod_av] fixed memory leak problem in encoding h264
Applying: temp silence warnings until we can resolve deprecation warnings on newer lib versions
Applying: FS-8959 #resolve refactor code
Applying: FS-8971 Resolve globals struct handling. Thanks to Ben Hood for reporting the issue.
Applying: FS-8959: fix var types to match the prototype
Applying: FS-8959: a bit more refactor of avcodec
Applying: FS-8959, FS-8186: fix build with --disable-libyuv
Applying: FS-8978: [mod_redis] Fix limit counter not decrementing on hangup
Applying: FS-8765 - [verto_communicator] Making preview button work again
Applying: FS-8977: Add support for NVENC H264
Applying: FS-8836 fix deprecated warning on newer ffmpeg
Applying: remove temporary warnings silence
Applying: FS-8750: fix variable set but not used warning
Applying: FS-8977: default to enable hw encoder on conference too
Applying: FS-8977: fix typo
Applying: FS-8982 #resolve
Applying: FS-8749 #resolve #comment please test
Applying: FS-8983 [avmd] Enable avmd on outbound channel
Applying: FS-8688 #improve vp9 processing to avoid chrome hang
Applying: FS-8918 #resolve [10 Second timeout after Notify during Proxy refer.]
Applying: FS-XXXX: [mod_av] tweak of parameters in h264 encoding
Applying: FS-8972 - [verto_communicator]  add i18n using angular-translate and static file loader
Applying: FS-7125 Added sofia event "wrong_calls_state". This is for fail2ban logging
Applying: FS-8972 - [verto_communicator] Fix mute Video translation
Applying: FS-8988 Rename files
Applying: FS-8990 - [mod_verto] - Adding verto_login header to verto::client_disconnect event
Applying: FS-8991 - [verto_communicator] Adding translations for fr_FR. Thanks Tristan Mahé.
Applying: FS-8989 - [verto_communicator] Adding Portuguese i18n translations to Verto Communicator
Applying: FS-8971 build fix. There are two different status variables with two different meanings. This splits them back apart.
Applying: FS-8991 - [verto_communicator] Adding translation for CANCEL for fr_FR
Applying: FS-8991 - [verto_communicator] Fixes for fr_FR and adding fr_CA
Applying: FS-8991 - [verto_communicator] minor change
Applying: FS-8933 WIP Fix some breakage on raspi as we dont want the FS repos there yet as we dont have armhf packages at this time
Applying: FS-8875 Enable faster beep detection
Applying: FS-8992 #resolve [Indicate end of candidates in SDP]
Applying: FS-8993 #resolve [Sync issues on conference playback of video that is faster frame rate than the conference]
Applying: FS-8995 - [verto_communicator] add missing toastr in settings controller
Applying: FS-8990 add verto_client_address to verto and presence events
Applying: FS-8996 - [verto_communicator] fix typo in CAMERA_SETTINGS id and add some italian translation
Applying: FS-8972: Added russian translation for Verto Communicator
Applying: FS-8997 - [verto_communicator] fix fallbackLanguage
Applying: FS-8998: [verto_communicator] add files for some more translations
Applying: FS-8998: [verto_communicator] add verto german translation
Applying: FS-8998: [verto_communicator] add spanish translation
Applying: [verto_communicator] Adding de, es, pl, ru, sv and id translations.
Applying: FS-8998: [verto_communicator] fix syntax error
Applying: FS-9000 #resolve [Set conference flags or conference member flags with individual variables per flag]
Applying: FS-9002 #resolve [rtp timeout code is parsed on video but its designed for audio]
Applying: add pcap-extract.sh
Applying: doh
Applying: FS-8998 add zh_CN
Applying: FS-8998 Oops, typo
Applying: FS-9005 - [verto_communicator] typo in label id
Applying: FS-8999: Fixed broken Erlang outbound connection
Applying: FS-8972: Minor fixies of russian translation of Verto Communicator
Applying: FS-8977: tweak nvenc params
Applying: FS-8913 #resolve [Problem with transfer when using bypass_media + SRTP + Inbound late negotiation]
Applying: FS-9012 - [verto_communicator] Fixing sidebar in narrow resolutions
Applying: FS-9013 #resolve [Add vad talk time logging channel vars]
Applying: spec file update for asm build requirements for libyuv
Applying: spec file update for asm build requirements for libyuv
Using index info to reconstruct a base tree...
M       freeswitch.spec
Falling back to patching base and 3-way merge...
No changes -- Patch already applied.
Applying: FS-9004 [mod_http_cache] added download-timeout param to prevent http_get from waiting unbounded time for downloading to finish.  Prevented prefetch threads from blocking if another thread is already downloading the same URL.
Applying: FS-9004 [mod_http_cache] set http get timeout on thread that is actively downloading with the value from the download-timeout configuration.
Applying: FS-9000 tweaks
Applying: FS-9000: fix compile on bsd and with libyuv disabled
Applying: [verto_communicator] FS-9015 Minor fixes in Polish translation
Applying: FS-9015: [verto_communicator] save in UTF-8
Applying: FS-9016 Avmd segfaults on NULL read codec
Applying: FS-8883: fix compile due to unused result failure on gnu compiler with --disable-debug
Applying: FS-8779: fix the include for Windows builds that point to in tree lib
Applying: FS-8780: fix the include for Windows builds that point to in tree lib
Applying: FS-8562: [mod_sofia] add update support for Mitel user agents
Applying: FS-8294: [freetdm] pass in modinstdir to freetdm configure
Applying: FS-8875: [mod_avmd] fix windows build from this change
Applying: FS-8978 part two
Applying: [avmd] FS-9019 Extend syntax description
Applying: [avmd] FS-9023 Add console auto completion
Applying: FS-9020 Avmd internal/external channel
Applying: FS-9027: [avmd] Check buffer
Applying: FS-9028: [avmd] Check SMA buffer
Applying: FS-9031: [avmd] Check session initialization
Applying: FS-9031: [avmd] Check session initialization
Applying: FS-9038: Add translations to support danish
Applying: FS-9036: [avmd] Cast to unsigned
Applying: FS-8623: fix sun studio error trying to compile char[] with c++ compiler
Applying: FS-8623: fix sun studio build errors building libvpx
Applying: FS-8623: fix sun studio build errors building libvpx
Applying: FS-8623: we really need this flag, but need to fix libvpx configure to let us pass it
Applying: FS-9025 - [mod_callcenter] bypass_media_after_bridge working for member channel
Applying: swigall
Applying: FS-9006 - [verto_communicator] add-combobox for languages
Applying: FS-9042: [core] fix assert when recording native file
Applying: bump apr build from api update
Applying: FS-9043 [mod_kazoo] add kz_export
Applying: FS-9039: [avmd] Use FS enumeration
Applying: FS-7317
Applying: FS-9049
Applying: FS-9052 [mod_hiredis] add connection pooling, improve dropped connection resiliency, and allow 0.10.0 of hiredis for CentOS 6.
Applying: FS-9054 [mod_hiredis] add ignore-connect-fail profile param so that calls do not get killed if limit fails due to lost connection
Applying: FS-9058 [mod_hiredis] allow auto decrement of non-interval limits on channel hangup.
Applying: FS-9059 [mod_hiredis] add session logging
Applying: FS-9058 [mod_hiredis] fix rate counters so the keys expire after interval completes.  Do not auto decrement rate counters.  Do not log null responses.
Applying: FS-9056 #resolve [Mobile H.264 video is black]
Applying: FS-9030, FS-9050: [avmd] Fix APP interface
Applying: FS-9072: [mod_syslog] Allow logging of messages with tab character
Applying: FS-9075 [Debian Packaging] #resolve freeswitch-all package rework
Applying: FS-9074: [mod_skinny] Fix incorrect location of free causing memory leak of xml when certain errors occur
Applying: FS-9077 [mod_verto] Adding verto_hangup_disposition variable to indicate who hangup
Applying: FS-8949 #resolve [switch_rtp.c Send end packet for [X] not recognised]
Applying: FS-9078 added hepv2 and hepv3 support
Applying: FS-9080 - [mod_spy] Making mod_spy work with Verto channels
Applying: bump version number in generated doxygen output
Applying: FS-9081 use turbo if available for newer jpeg over falling back to old jpeg62-dev
Applying: FS-9078 added #pragma for MSVC compiler
Applying: FS-9081 to build all modules for trusty needs the universe components
Applying: FS-9081 Correction to e8f83d0
Applying: FS-9083 [mod-sofia] Pass On SIP headers from leg A to B
Applying: Buffer overflow in switch_channel_expand_variables_check and switch_event_expand_headers_check fixed (FS-8757)
Applying: FS-9082: [mod_java] coreectly reference the modules dir so mod_java can load what it needs when modules are not placed in prefix/mod directory
Applying: one push one pop
Applying: FS-9024: [avmd] Add events
Applying: FS-9057 #resolve [Screen Share feed no longer takes floor when someone mutes and unmutes their webcam]
Applying: FS-9085, FS-9024: [avmd] Add events
Applying: FS-9091: update libyuv to hash 69245902 from https://chromium.googlesource.com/libyuv/libyuv/
Applying: FS-9091: build all libyuv platform files so we don't have missing symbols on some platforms
Applying: FS-9060: [mod_sofia] correct issues with hold and broken soa negotiations after performing a bypass media reinvite
Applying: FS-9093: mod_cv: remove unneeded includes
Applying: FS-9075 fixup for systemd and sysvinit
Applying: FS-9099 #resolve [Websocket raw frame read timeout is too short]
Applying: FS-9062 #resolve [OPUS - Mid Call change from 20ms to 40 ms causes jittery voice ]
Applying: FS-9076 #resolve
Applying: FS-9075 [deb packaging] tweak freeswitch-meta-all dependancies to more fully install FreeSWITCH.
Applying: mod_html5 has been long depricated. this is a dead package
Applying: FS-9075 [deb packaging] removing some meta-all dependancies that are causing issues
Applying: FS-9075 [deb packaging] removing some more meta-all dependancies that are causing issues
Applying: FS-9106 #resolve [vpx performance tweaks]
Applying: FS-9106 up default FPS to 30
Applying: FS-9075 [deb packaging] tweak dep for freeswitch-init
Applying: Revert "FS-8704: Add min-members, wait-min-members-timeout, wait-mod-timeout, wait-min-members-timeout-message, wait-mod-timeout-message, endconf-mod-exit-message, and endconf-message parameters and functionality to mod_conference."
Applying: FS-9086 #resolve [Video files playing in mod_conference do not count in totals for calculating layout]
Applying: FS-9106 add new version of previous vpx sleep patch
Applying: FS-9078: [sofia-sip] fix windows build of HEPv2/HEPv3 code
Applying: FS-9099: [sofia-sip] fix windows build of websocket transport
Applying: FS-9099: remove unneeded header include
Applying: FS-9078: [sofia-sip] fix linux build of HEPv2/HEPv3 code
Applying: .update
Applying: FS-9078: [sofia-sip] fix linux build of HEPv2/HEPv3 code
Applying: FS-9099: [sofia-sip] fix windows build of websocket transport
Applying: FS-9078: [sofia-sip] fix typo in HEP3
Applying: FS-9011 [avmd] Add XML config
Applying: Update debian/control-modules for new modules
Applying: Ignore debian/freeswitch-init.provided_by in git
Applying: FS-9109: [build] attempt to fix misleading indentation errors on gcc 6.0
Applying: FS-9100 #resolve [Recording Fails if There Are Zero Webcams]
Applying: FS-9099: fix windows build
Applying: Revert "FS-9081 Correction to e8f83d0"
Applying: Revert "FS-9081 to build all modules for trusty needs the universe components"
Applying: FS-9081 [Ubuntu Packaging] WIP Patches to build system and configure to allow FS to build on 14.04
Applying:  FS-9109
Applying: FS-9117 [avmd] #fix build on Windows
Applying: FS-9113 [sofia-sip] Clear out ssl error queue
Applying: FS-9079: [mod_callcenter] Add ring-progressively strategy
Applying: version bump
Applying: FS-5936 ESL.pm packaged for Debian
Applying: FS-9115 #resolve #comment trying to support audio only mp4 recording, please test
Applying: FS-8795 #resolve #comment trying support audio only call in mod_png, please test
Applying: FS-5936 wrong dependency on freeswitch-mod-esl
Applying: FS-9070 Update config.sub and config.guess
Applying: FS-9124 [avmd] Extend XML config
Applying: tweak fscore_pb to use new pastebin API
Applying: FS-9131: improve validation of ice candidates
Applying: FS-5936 respect archlib in libesl-perl because it is different depending on distro
Applying: FS-9075 [Debian Packaging] futher tweaks to help ease upgrading freeswitch-all
Applying: FS-9135: handle null sdp sent to switch_core_media_set_sdp_codec_string
Applying: FS-9135: handle null sdp sent to switch_core_media_set_sdp_codec_string
Applying: Properly handle NULL var_name for switch_play_and_get_digits
Applying: FS-9132: [mod_kazoo] Add more vars to default filter
Applying: FS-9132: [mod_kazoo] remove whitespaces
Applying: Cleanup inconsistent whitespace in debian/util.sh
Applying: Remove superfluous semicolon
Applying: FS-8623: Fix libvpx Solaris Studio build
Applying: FS-9151 #resolve
Applying: FS-9155 [Centos Packaging] fix lang_es and lang_pt package to have the right language module
Applying: FS-9152 [avmd] #fix warnings on FreeBSD
Applying: FS-9010 [avmd] Dynamic passing of parameters
Applying: FS-9148: add new voicemail.conf.xml param `send-full-vm-header`
Applying: Revert "FS-9148: add new voicemail.conf.xml param `send-full-vm-header`"
Applying: FS-9148: add new voicemail.conf.xml param `send-full-vm-header`
Applying: FS-9157 [verto] Added possibility to use dedicated audio/video tags for each dialog
Applying: FS-9157 update jsmin
Applying: Incrementing Verto version in package.json
Applying: FS-8584 - [mod_callcenter] Requesting agents and tiers when reloading queue
Applying: FS-9158 [sofia-sip] Add include for changes in 65460fa
Applying: FS-9153 #resolve [uuid_bridge issue on ESL]
Applying: FS-9164: [core] add Session-Per-Sec-Last to heartbeat event
Applying: FS-9034 revert commit in sofia.c that prevents register in new thread
Applying: FS-9167 #resolve [Playing file when all video feeds are vmuted does not show file]
Applying: FS-9153 #resolve [uuid_bridge issue on ESL]
Applying: FS-9160 #resolve tweak sip_invite_failure_* chan vars for properly reporting last outbound call failure when there are multiple bridge attempts on a single call
Applying: remove extra condition code that can never be called
Applying: FS-9185: fix format ifdefs for Solaris SPARC
Applying: [mod_sofia] [FS-9188] added channel var to suppress auto-answer notify
Applying: FS-9184: Allow show calls to be filtered by accountcode
Applying: FS-8652 handle early-only param in replaces header
Applying: FS-8979 #resolve #comment please test
Applying: debian: fix compatibility with systemd 215 on Jessie
Applying: FS-9198 [mod_skinny] fix small memory leaks
Applying: FS-9106 followup and tweaks on this patch after some testing
Applying: FS-9201 [mod_skinny] fix leak in api call to list devices
Applying: FS-9202 [mod_skinny] fix leak in speed dial
Applying: FS-9198 #resolve [Small memory leaks in mod_skinny]
Applying: FS-9199 easier memory allocation debugging and analysis
Applying: FS-9207 #resolve [Add ignore_sdp_ice=true to ignore ICE when parsing an SDP]
Applying: A-law idle byte was defined incorrectly.
Applying: FS-9142 [avmd] Dynamic settings
Applying: FS-9212: [mod_conference] fix conference recording api when using default canvas number
Applying: FS-9150 #resolve ["video-mute-exit-canvas" param combined with "video-bridge-first-two" causes video feed to flip back and forth between feeds when users video-mute]
Applying: FS-9216: [mod_sofia] Add Cisco SPA30X and Grandstream GXP user agents to send UPDATE
Applying: FS-9174 #resolve add dep to meta-all for mod_png so its installed via the debian -all packages
Applying: FS-9156: Code Improvement for the non-interval increment when limit reached
Applying: FS-9222 small tweak to freeswitch console to strip leading spaces from commands
Applying: FS-9197
Applying: FS-9197
Applying: FS-9197
Applying: FS-9225: [mod_sofia] Allow to force SIP REGISTER Expires: to be within configured range.
Applying: FS-9136: allow multiple instances of same video codec with different fmtp
Applying: FS-9136: remove unused vars
Applying: FS-9136: update other modules to match api change
Applying: FS-9142 [avmd] Dynamic settings
Applying: swigall
Applying: FS-9235: Fix sending RTCP in switch_core_media
Applying: FS-9214: fix 3pcc behavior
Applying: FS-7397: fix segfault due to memory corruption on using mod_translate app.  The session pool was being freed incorrectly after using the app instead of when the session pool was destroyed
Applying: FS-9227: [sofia-sip] fix Wrong byte order in HEP packet for source and destination ports
Applying: FS-9219 #resolve [Re-INVITE with no SDP ] #comment use bypass_media_after_bridge_oldschool=true to enable back-compat bypass media after bridge
Applying: FS-9238: [mod_osp] Updated for OSP Toolkit 4.11.3.
Applying: FS-8979 #comment fire event when done
Applying: FS-9144 #resolve [Implement video-mute-exit-canvas and recording in personal-canvas mode.]
Applying: FS-9247 #resolve [Table with message type names not updated when enum was updated by sangoma patch]
Applying: FS-9248 [mod_callcenter] Adding truncate-tiers-on-load and truncate-agents-on-load options.
Applying: FS-9249 [verto_communicator] Closing settings panel if the user clicks outside the element
Applying: FS-9249 [verto_communicator] dedicated function to close settings
Applying: FS-9250 [verto_communicator] Putting factory reset button back
Applying: FS-9246 #resolve [No Audio after Call transfer ]
Applying: FS-9244 #resolve [RFC2833 payload_type offered is ignored]
Applying: FS-9254: [avmd] fix windows build
Applying: bump version
Applying: FS-9256: mod_v8: Add DB.Finalize() in order to close statements.
Applying: FS-9214 regression
Applying: FS-9244 fix debug lines
Applying: FS-9265 #resolve [INCOMPATIBLE_DESTINATION when there is no rtcp]
Applying: FS-9266 #resolve
Applying: FS-9192-proxy-hold: proxy hold when proxy media and proxy mode are disabled; its similiar to proxy-refer
Applying: FS-9271: [mod_conference] fix segfault trying to record a canvas that does not exist
Applying: FS-9267 #resolve [Raw decoded image from vpx codec is corrupted by video media bugs that modify the image]
Applying: FS-9264: Introduce two new api calls named detect_audio and detect_audio_silence. The existing wait_for_silence call never actually waits for silence until it first detects non-silence. There is also no way to set an independent timeout for detecting both the non-silence and then silence. This causes problems when wait_for_silence is called on an already quiet channel. Splitting the function up into two separate calls with separate timeouts offers more flexibility. Applying: FS-9260: fix make detection to not fail on openbsed, and fix libtoolize detection to attempt to find libtoolize the same version as specified libtool
Applying: FS-9260: [build] add -ltermcap for openbsd so it can correctly link to libedit
Applying: FS-9263: [build] attempt to find the proper lua5.2 version on openbsd
Applying: FS-9283 --resolve
Applying: FS-9287 Add channel variable to make spandsp_start_tone_detect easier to use from dialplan / embedded scripts.
Applying: FS-9292 #resolve [Core dump playing videos or show images]
Applying: FS-9292 #resolve [Core dump playing videos or show images]
Applying: FS-9296 #resolve [Video support for mod_httapi]
Applying: FS-9297: [mod_sofia] fix multiple crashes from passing invalid null values in sofia.conf
Applying: fix typo in conference config
Applying: FS-9301: [mod_sofia] handle race condition on startup of mod_sofia in error conditons causing segfault
Applying: FS-9302 [mod_mongo] mongo_find_one and mongo_find_n corrected to return -ERR when connection to database fails.
Applying: FS-9221 Add inactive support
Applying: FS-9303 add CONF_VIDEO_MODE_NONE so we don't default to CONF_VIDEO_MODE_PASSTHROUGH
Applying: Revert "FS-9303 add CONF_VIDEO_MODE_NONE so we don't default to CONF_VIDEO_MODE_PASSTHROUGH"
Applying: FS-9303 these checks are no longer needed as the video flag is not sent to file open unless we are in transcode mode, you can record mp4 but it will only contain the audio if in passthru mode.
Applying: FS-9305: [mod_conference] return the logo image path from video-logo-img api and handle passing no image path
Applying: FS-9307: [mod_conference] don't close files until after video threads are done to avoid race condition trying to use closed file handle when playing a video file
Applying: Fix for FS-9313
Applying: FS-9312 #resolve [unreachable code block in switch_core_media]
Applying: FS-9314: [mod_conference]  fix crash when starting conference in mux mode while specifying or defaulting to a layout group that does not exist.  We will now fall back to transcode mode in this case.
Applying: FS-9317 add screen share examples
Applying: FS-9320 #resolve [Adjust buffering based on frame rate differential]
Applying: FS-9278: sending sendonly instead of sendrecv media attribute in SDP when unholding call when Zoiper softphone had responded with inactive
Applying: FS-9315 [mod_http_cache] add support for video file formats
Applying: FS-9009 [mod_avmd] Amplitude estimation
Applying: FS-9241 Use tls_public_url instead of tls_url in INVITE Contact when NAT is detected
Applying: FS-9267 fix regression
Applying: FS-9241: follow-up patch to fix a mistake in fetching profile url
Applying: FS-9316 #resolve [INVITE with empty SDP from Cisco VCS cannot setup video]
Applying: FS-9328 #resolve [switch_jb_peek_frame uses wrong len]
Applying: FS-9333 #resolve [Disable video refresh by sip INFO by default]
Applying: FS-9310 Native support for Flowroute SMS API over HTTP(S)
Applying: FS-9310 Part two. Properly destroy the timeout struct after the message has sent, or timed out.
Applying: FS-9310 Fix RPM build due to new config file. Also add libvpx generated file to .gitignore
Applying: FS-9334 #resolve [Jitterbuffer mods]
Applying: FS-9009 [mod_avmd] #fix build on Windows
Applying: FS-9009 [mod_avmd] #fix warning on Windows
Applying: FS-9337: fix invalid sdp generated with soa disabled
Applying: Revert "Fix for FS-9313"
Applying: FS-9342 - [verto_communicator] Properly saving settings to localStorage when closing the settings panel.
Applying: FS-9345 #resolve [HTTAPI truncates string when response spans multiple packets]
Applying: FS-9343 #resolve [Sending a Message using the mod_smpp module via Nexmo Fails]
Applying: FS-9350: add mod_av commented to modules.conf.xml
Applying: FS-9352 #resolve [ptime adjust issues on opus]
Applying: FS-9355: fix segfault in case of null frame
Applying: FS-9356 #resolve [DTMF not recognized when coming from a Cisco SIP trunk]
Applying: FS-9353 #resolve [clear-vid-floor produces error, while working]
Applying: FS-9259 #resolve [Missing "m=image 0" when replying to INVITE with disable image line]
Applying: FS-9289 #resolve [MOH issue]
Applying: FS-9365 #resolve [SDP Format on reply to RE-INVITE does not appear to be RFC-4566 compliant]
Applying: FS-9230: Customize video muted banner
Applying: Simple fax test script
Applying: clean up noisy configure
Applying: FS-9357 #resolve [VP9 codec screenshare on mod_conference (mux/transcode) does not work]
Applying: FS-9342 [verto_communicator] Only copy data to storage when closing the settings
Applying: FS-9334 revert
Applying: FS-9373 #resolve make add mod-verto and mod-rtc to freeswitch-meta-all debian package
Applying: FS-9368
Applying: FS-9334
Applying: FS-8783: [libsrtp] Fix alignment issue
Applying: FS-9376 #comment please try this
Applying: FS-9357 cleanup and tweak debug
Applying: FS-9357 handle packet loss, reset decoder on mem err
Applying: FS-9381: [mod_sofia] fix leak in sofia_presence_chat_send
Applying: do not destroy conference if ghost(s) are present, add ghost count check before setting flag
Applying: FS-9382
Applying: FS-9386: [mod_snmp] Use net-snmp-config for SNMP libs if available
Applying: FS-9154 #resolve [Add & remove video on re-invites]
Applying: revert FS-9368
Applying: FS-9390 #resolve ['Segmentation fault' during call setup]
Applying: FS-9369: [core_media] add_ice_candidates=true var to enable inserting ice candidates in outgoing sdp
Applying: FS-9394 #resolve fix h263 leak
Applying: FS-9276 new feature proxy in-dialog calls sip notify and info similar to proxy hold
Applying: FS-9380: switch_core_media.c , fix_ext-rtip-ip_not_used_when_originating
Applying: FS-9380: switch_core_media.c , cleaned from logging
Applying: FS-9401: [core,mod_amqp] fix leak in usage of hash itterator
Applying: FS-8761 #resolve [Memory leak in FreeSWITCH]
Applying: FS-9403 #resolve [add timestamp for when user was pushed into queue that lives with the channel]
Applying: Revert "FS-8761 #resolve [Memory leak in FreeSWITCH]"
Applying: FS-8761 #resolve [Memory leak in FreeSWITCH]
Applying: FS-9409: Wait for avformat reader thread before reading.
Applying: FS-9069: [mod_avmd] add detection time to beep event
Applying: FS-8761
Applying: FS-9415 [mod_spy] - Increasing loop so we can also look for variable_verto_user and variable_verto_host
Applying: try to deliver locally to verto too
Applying: tweak on yesterday's commit
Applying: FS-9419 #resolve [Add event_channel_broadcast api]
Applying: FS-9183: [mod_sofia] Handle 415 Unsupported Media Type as 488
Applying: FS-9424 #resolve Define byte order correctly on Solaris/SPARC
Applying: FS-9422 #resolve [Freeswitch Exit/Crash on SDP Negotiation] #comment renegotiate-codec-on-hold renegotiate-codec-on-reinvite are both removed in this commit
Applying: FS-9410 #resolve [PLI Missing Media Source SSRC]
Applying: FS-9423 #resolve Handle null value in ACL list name
Applying: FS-9161: add example Verto settings to example configs
Applying: FS-9281: Add support for QQVGA resolution in Verto
Applying: FS-9375 #resolve [DTMF not working on OPUS after Call Transfer ] #comment Can we try to reproduce with this version on all 3 boxes (not just the patch but the whole rev as-is)
Applying: FS-9362: [mod_sofia] fix sofia compile error on newer clang included in new osx
Applying: FS-9398 solve missing variables in sofia events expire and unregister
Applying: FS-9434 #resolve [SDP parser in sofia does not recognize UDP/TLS/RTP/SAVP]
Applying: FS-9425 fix copy and paste error where we were not setting the height properly.
Applying: FS-9436 #resolve [RTCP PLI Media Source SSRC wrong after re-INVITE]
Applying: FS-9437 #resolve [Delete avatar if video is enabled mid-call]
Applying: FS-9440 add transfer_destination
Applying: FS-9441 optional skip member outcall beep
Applying: Revert FS-8565, its getting a few blames for 3cx interop issues, we need to revisit the original reason and make sure this was the right fix.
Applying: Fix compiler warning/error in ftmod_r2.c
Applying: FS-9447: [mod_avmd] increase default value of samples to skip
Applying: FS-9447: [mod_avmd] increase factory default value of samples to skip
Applying: FS-9439 check chained loopback for loopback_bowout
Applying: FS-9449: Enable clock calibration and clock_realtime on Solaris
Applying: FS-9443 #resolve [SDP in a verto.invite with missing ICE candidates can segfault]
Applying: FS-9447: [mod_avmd] #fix PRId64 on windows
Applying: FS-9143 [avmd] #fix event headers
Applying: dont build mod_flowroute_sms as there is not any libh2o packages at this time
Applying: FS-9452: fixed true/false logic for using dst flag
Applying: add sysvinit-utils dependancy for ubuntu to debian bootstrap.sh
Applying: FS-9442 #resolve #comment tweak the deb packages to properly install the debug symbols via freeswitch-all-dbg and freeswitch-meta-all-dbg
Applying: FS-8608 found while doing config audit, params should have dashes
Applying: FS-8608 remove nonexistant option, found during config audit
Applying: Revert "FS-9442 #resolve #comment tweak the deb packages to properly install the debug symbols via freeswitch-all-dbg and freeswitch-meta-all-dbg"
Applying: FS-9442 #resolve #comment tweak the deb packages to properly install the debug symbols via freeswitch-all-dbg and freeswitch-meta-all-dbg lang packages do not have dbg packages as they are just xml
Applying: FS-7706 [mod_callcenter] Hangup agent channel if we failed to bridge it with member channel.
Applying: version bump
Applying: FS-9404 Handle sequence rollovers in mod_av handling of inbound H.263.
Applying: [ubuntu packages] FS-9465 #resolve Add xenial instrumentation to debian/utils.sh script
Applying: The band filter for G.722 could cause numerical overflow in unusual circumstances with the maximum possiblke signal level. The filter output is now saturated to 16 bits to avoid this.
Applying: FS-9469
Applying: FS-9471 [verto_communicator] Updating In Call display after receiving display update message from mod_verto.
Applying: FS-9466: Use system MD5 if available
Applying: FS-9472 [core] Add originate_retry_timeout and originate_retry_min_period_ms
Applying: FS-9474 #resolve [Add variables to set initial volume on mod_conference]
Applying: remove debug
Applying: FS-9475 #resolve [Video bandwidth not conveyed in SDP for verto]
Applying: update display in vc
Applying: FS-9138: [avmd] Add config to vanilla folder
Applying: FS-9480 [mod_kazoo] add api enhancements
Applying: FS-9482 #resolve [uuid_media_3p - seg fault on 2nd attempt]
Applying: FS-9483 #resolve [mod_conference missing keyframe after reinvite]
Applying: FS-9551 [mod_sofia] compare also session before setting TFLAG_SKIP_EARLY
Applying: FS-9457 [mod_http_cache] Allow GET and PUT from Azure Blob Service
Applying: FS-9457: fix code after decl error
Applying: FS-9457: fix code after decl error
Applying: FS-9457: fix code after decl error
Applying: FS-9457: fix code after decl error
Applying: FS-9457: fix code after decl error
Applying: FS-9487 #resolve [Add CBR param to video file recording params]
Applying: FS-9488 #resolve [Compile error mod_http_cache]
Applying: FS-9484: fix var type format spec
Applying: FS-9493 #resolve [Possible crash when changing from normal to personal canvas on the fly]
Applying: FS-9494 #resolve [Issues with video avatar switching when video starts/stops]
Applying: FS-9486 #resolve [uuid_drop_dtmf switch between tone replace and digit]
Applying: FS-9495 #resolve [add conference_join_energy_level variable]
Applying: FS-9458: [avmd] Set channel variable before BEEP event is fired
Applying: FS-6954: Use channel flags to check for proxy media or bypass media
Applying: FS-9346 [verto_communicator] Add DTMF icon while on a video call, fixing conferences with pin number
Applying: FS-9497 #resolve [AV sync record issue]
Applying: FS-9498 #resolve [Try to make video writing thread more efficient]
Applying: add a script to configure and build FS as if it were installed from debian packages
Applying: FS-8761
Applying:  FS-9496
Applying: FS-9503 #resolve [Add flaws and consecutive_flaws to error_log in rtp]
Applying: FS-9503 messed up case
Applying: FS-9505 [mod_conference] honor verbose-events in conference-create event
Applying: FS-9506 #resolve [Proxy-Hold improvement, Support a=inactive]
Applying: FS-9506 code was too over-zealous about taking control when it should not, pass 1
Applying: FS-9509: [avmd] Add perl script subscribing to avmd events
Applying: FS-9506 code was too over-zealous about taking control when it should not, pass 2
Applying: FS-9511 #resolve [Sync issues on inbound video calls]
Applying: FS-9518 [mod_conference] allow deaf only command in caller-controls
Applying: FS-9511 up the max size a tad
Applying: FS-9519: [avmd] Add unit test
Applying: FS-9424: Make big endian ifdefs more specific
Applying: FS-9522 #resolve [Add rtp_assume_rtcp to always use rtcp when needed]
Applying: FS-9533 [mod_conference] add member-enter-sound
Applying: FS-9538 #resolve [segfault while reading local ringback file]
Applying: FS-9526 [mod_conference] add deaf sounds
Applying: FS-9527 [avmd]: Fix MAP_POPULATE on FreeBSD
Applying: FS-9548: return with error on wrong rtp ip given from config
Applying: FS-9543 #resolve [Add pre-exec state change hooks to core]
Applying: FS-9549 #resolve [Add userVariables to DMTF and INFO messages]
Applying: FS-9524 #resolve [Enable whitelisting of Verto connections by IP using FS ACL]
Applying: FS-9550 #resolve [Set user on outbound verto calls to sync with user directory]
Applying: FS-9551 [switch_ivr - json cdr] Adding app-stamp to app_log
Applying: FS-9552: [mod_conference] added 'deaf' to the json status per member
Applying: FS-9435 #resolve [PLI requests once per second]
Applying: FS-9536 [core] fix return value
Applying: FS-9498 fix regession with 100% cpu
Applying: FS-9522 fix regression
Applying: FS-9525 #resolve [Client initiated REINVITE with different audio codec into conference causes choppy audio]
Applying: FS-9557 #resolve [Eating AV in proxy media mode]
Applying: FS-9557
Applying: FS-9525 part 2
Applying: update to libvpx b46243d from repo https://chromium.googlesource.com/webm/libvpx
Applying: FS-8623: reapply after update: fix sun studio build errors building libvpx
Applying: FS-8623: reapply after update: Fix libvpx Solaris Studio build
Applying: newest version of sleep patch
Applying: FS-9522 more regression
Applying: FS-9522
Applying: update libyuv to hash bcd8238 from https://chromium.googlesource.com/libyuv/libyuv/
Applying: FS-9553 #resolve [Refactor video-on-hold]
Applying: FS-9574 #resolve [We shouldn't print data sent on the buffer.]
Applying: FS-9548 #resolve [crash on Invite due to bad config for sip profile ]
Applying: FS-8644: OPUS_SET_BITRATE(), codec control and estimators for packet loss and RTT (with Kalman filters) to detect a slow or congested link.
Applying: Bump Version Number
Applying: FS-9242 convert to adapter.js
Applying: FS-8955 [verto_communicator] Adding DTMF shortcuts and handling DTMF history on DTMF widget
Applying: FS-9508 [verto_communicator] Adding AGC option on settings, enabled by default
Applying: FS-7876 [verto_communicator] Adding hold button for video calls
Applying: FS-9242 fix screen share for chrome to work in VC with additional camera
Applying: FS-9580 #resolve [RTCP Auto-adjust] %backport=1.6
Applying: FS-9586 #resolve [local_stream video queue stuck when not being read from] %backport=1.6
Applying: update config file
Applying: FS-9552 add to verto communicator toggle deaf status button
Applying: FS-9584 Separate initial bitrate negotiation from sample rates
Applying: FS-9552
Applying: FS-9552
Applying: FS-9593 #resolve [Video syncs too much on video muted channels] %backport=1.6
Applying: FS-9597 #resolve [host_lookup does not resolve v6 addrs] %backport=1.6
Applying: FS-9596 #resolve [rtp-timeout triggered for on-hold calls with a=inactive]
Applying: FS-9601: mod_opus:  make adjustable bitrate mutually exclusive with FEC enforcing on the decreasing trend,
Applying: FS-9610 #resolve [Video keyframe requests not being propagated properly] %backport=1.6
Applying: FS-9612 #resolve [RTCP-MUX wrongly enabled in cases where answer contains rtcp but offer didn't / remote addr not obtained in UDPTL mode] %backport=1.6
Applying: FS-9612
Applying: bump version
Applying: FS-9639 e3353b7 backported
Applying: FS-9648 #resolve [Conference avatar image gets stuck enabled when it uses the same image as video mute]
Applying: FS-9642 #resolve do not limit time for real time threads with systemd. this causes systemd to randomly restart FreeSWITCH without notice as to why. %backport=1.6
Applying: FS-9653 #resolve freeswitch-meta-all-dbg should not depend on non-existant freeswitch-meta-lang-dbg %backport=1.6
Applying: FS-9654 #resolve [Issue with RTP payload negotiation]
Applying: swigall
Applying: FS-9666 #resolve [Remove legacy code when getting an XML video refresh]
Applying: FS-9666 fix unused variable
Applying: FS-9654
Applying: FS-9654: Fix RTP packet drops
Applying: FS-9685 Update broadsoft SLA to work with newer Polycom firmware.
Applying: FS-9455 #resolve [Doubled posts in the chat window ]
Applying: FS-9699 #resolve [Improper response to reinvite after using uuid_media_3p]
Applying: FS-9628: [verto] correct return value of $.FSRTC.bestResSupported to be the largest resolution
Applying: FS-9623 update .update
Applying: FS-9635: update deprecated stream.onended
Applying: FS-9395: downgrade some FSRTC console.error messages
Applying: FS-9594: Has been adjusted string vars size to fit IPv6 address
Applying: FS-9594: Fixed hostname resolving for IPv6 addreses.
Applying: FS-9590 check dtmf_type variable when negociating inbound SDP
Applying: FS-9640: Allow access to Verto stream object via callback
Applying: mod_sofia: Add 'username' header for expiration events.
Applying: FS-9632 remove SWITCH_FILE_FLAG_VIDEO flag if we fail to receive video so we fall thru and record the audio only.  Previously it would just fail to function as expected.
Applying: FS-9649 Fix filebug script if EDITOR env variable not set
Applying: FS-9650 initialize pointer to NULL to resolve warning
Applying: FS-9651 Fix incorrect expression
Applying: FS-9647 Improve spanish translations
Applying: FS-9643 mod_opus Log encoded stats at end of call
Applying: FS-9656 Coverity 1364971: resolve memory leak in new HEPv3 logging
Applying: FS-9634 #resolve [RTCP mux is always proposed on outbound channels even if rtcp_mux chan variable is 0]
Applying: FS-9660: Fix BW calculation for SDP media clause
Applying: FS-9151 FS-9631 #resolve
Applying: FS-9634: fix typo
Applying: FS-9665 #resolve [Add video_pre_call_banner feature]
Applying: FS-9662 #resolves [mod_opal] Fix version number in old OPAL error
Applying: FS-9646 [verto_communicator] Fixing settings toggle
Applying: FS-9668 #resolve [Add proxy-info feature]
Applying: FS-9680 #resolve [Add proxy-message param to sofia]
Applying: FS-9678: Fix FreeSWITCH not shutting down when profiles fails to load and shutdown-on-fail is set to true
Applying: FS-9629: add isfocus to replies, and add is_conference support to pre_answer
Applying: Commit an example of using the IVRMenu object to build the ivr in lua.
Applying: FS-9687: segfault Program terminated with signal SIGFPE, Arithmetic exception.
Applying: FS-9714: [core] switch assert on attempted double close of a file handle
Applying: FS-3568 #comment update Chinese phrase XML, thanks Gaofei
Applying: FS-9668 also check for keyframe requests on INFO even when proxying for good measure
Applying: FS-9697 #resolve [Video compat changes to support older clients]
Applying: FS-9700 #resolve
Applying: FS-9704 #resolve [Seeking video files with mod_av after it reaches the end does not work]
Applying: FS-9705 #resolve [Files using prebuffer do not play properly when seeking back to the beginning once the file is done playing]
Applying: FS-9706 #resolve [Add loops param to file playback in conference]
Applying: FS-9693 use existing date parsing functions in fulldate comparison that take timezone into account
Applying: FS-9715 #resolve [Add support for m4a]
Applying: FS-9709 #resolve [JB drops packets after hole-punching]
Applying: FS-9719: Separate gen_ts_delta between audio and video and enable support to auto engage this feature for pass-thru video
Applying: FS-9714: [mod_conference] fix crash due to race on closing file handles when playing a file to a conference via api command as a conference is shutting down
Applying: FS-9204: complte the urls so that snom can execute the pickup, It used to probably send it to the proxy, but now needs the host in the packet or throws network error
Applying: FS-9725: Disable blank img with core_video_blank_image=false.
Applying: FS-9726 Fix malformed PAI
Applying: FS-9712 #resolve [3PCC-Proxy Missing SDP on Reinvite. ]
Applying: FS-9691: don't call sql code inside hash_mutex due to circular mutex contention between hash_mutex and profile->dbh_mutex
Applying: FS-9671 fix wrong cseq in notify Expires 0
Applying: FS-9592: [mod_httapi] make sure to reset one time params when starting over in httapi app
Applying: FS-9652: improve sql sanitization
Applying: FS-9728 fix dynamic media tag on verto.newCall method
Applying: FS-9727: raise size limit on unkown size odbc column data from 256 to 16k
Applying: FS-9412 [switch_loadable_module.c] Make MODULE_UNLOAD event report module name (key) and file (filename), similar to event MODULE_LOAD
Applying: FS-9387 [libzrtp] bugfix for hash calculation of the auxiliary secret
Applying: [FS-9367] log session counts at channel state change
Applying: FS-9715 add webm and mkv too
Applying: FS-9732 [mod_ssml] create default configuration so ssml.conf.xml is not needed
Applying: FS-9733 [mod_rayo] prevent bad tts format string from being generated when MRCP headers are not present
Applying: FS-9737 [mod_hiredis] fix limit_usage when using hiredis backend
Applying: swigall
Applying: update verto-min.js
Applying: FS-5978 [mod_rayo] attempt to fix direct media join
Applying: FS-9739 [switch_rtp mistakes "ffffffff" for a new DTMF and Queue's a NULL digit]
Applying: FS-9748: [Locking contention with mod_shout playing conference moh]
Applying: FS-9755 conference cdr is required for 4579 support in mod_conference
Applying: FS-9747: [core] add channel hold/unhold verbosity
Applying: version bump
Applying: FS-9851: [freeswitch-core] Add abstimeout to CoreSession:getDigits in switch_cpp #resolve
Applying: FS-9782: [mod_sofia] on recovery, don't flip the order of the record route ever, on outbound calls use the record route in the reverse order as the initial route set when doing the recover invite
Applying: FS-9782: [mod_sofia] on recovery, flip the order of the record route on inbound calls only, use the record route in the same order on inbound calls and in reverse order on outbound calls as the initial route set when doing the recover invite.  Account for the call direction based on how sip considers it, not based on freeswitch direction so inbound calls after recovery are treated as outbound in this logic
Applying: FS-9581: [windows] DEBUG_ESTIMATORS symbol conflict in windows
Applying: FS-9581: [windows] don't define symbols before pch include
Applying: FS-9581: add new required source files to core build
Applying: FS-9581: [windows] fix function signatures in switch_estimators.c
Applying: FS-9758: switch_sql_queue_manager_destroy() avoid null pointer deref
Applying: Remove arg limit
Applying: FS-9735 - send unknown headers to switch_ivr_set_user
Applying: FS-9780 [spandsp] Change MAX_COMMAND_TRIES to 6
Applying: FS-9787 #resolve remove duplicated headers in conference del-member events
Applying: tweak our bashrc %ignore
Applying: FS-9800: [core] add new accessor functions to core statistics for use in modules
Applying: FS-9801 fix incorrect xml sections for phrase files
Applying: FS-9277: sip info with record: on and off doesn't start and stop call recording sessions
Applying: FS-9799 rotate and increase log file size
Applying: FS-9804 broken closing tag
Applying: FS-8733 FS-9804 various phrase updates
Applying: FS-9794: Set the result cause of an originate failed cause to variable originate_failed_cause
Applying: FS-9788: Add close() option to FileIO implementation
Applying: FS-9812 fix label that is only used when zrtp or srtp are enabled
Applying: FS-9805: Created script to compare XML traslation files with phrase_en.xml
Applying: FS-9805: Organized prompts in phrase_en.xml
Applying: FS-9813 [mod_kazoo] add kz_http_put
Applying: FS-9817 #resolve fix regression from 828d6eaf0177caff9b60052be3c83b2008f85416
Applying: FS-9816 yasm is not a runtime dependency, its only needed to build libvpx when compiling
Applying: FS-9826 reset jitter buffer if SSRC changes regardless of jitter buffer paused state
Applying: FS-9824 [tone2wav.c] Fix segfault on tone2wav
Applying: FS-9821 [mod_lua] Fix. memory/resource leak in mod_lua
Applying: FS-9827 [mod_hiredis] handle NIL reply
Applying: FS-9808 #resolve [mod_png issue when used with ivr as video input]
Applying: FS-9843 [mod_avmd] Remove unused defines
Applying: FS-9829 #resolve [FreeSWITCH 200ok to second reINVITE on a dialog doesn't contain an SDP.]
Applying: %ignore update fixbug.pl to include component
Applying: fix typo %ignore
Applying: FS-9725: Fix echo if blank image is disabled.
Applying: FS-9863: [] video_width/video_height unset with playback application #resolve
Applying: FS-9745: [mod_sofia] Call to FS WebRTC Gateway fails when no SDP on invite #resolve
Applying: FS-9846: [mod_sofia] Bugs related with Hold and Proxy Hold option added in FS-9192 after merges in 1.6.11 #resolve
Applying: FS-9858: add configure switches to disable libpng and freetype support
Applying: FS-9866: [freeswitch-core] 3pcc=proxy for FS client and local SDP #resolve
Applying: FS-9760 Removed the un-needed whitespace from the file
Applying: FS-9869 [mod_callcenter] Exporting cc_queue_joined_epoch when originating agent outbound leg
Applying: FS-9870: [freeswitch-core] playback_timeout_sec does not stop a delimited playback #resolve
Applying: FS-9871: [freeswitch-core] DTMF not delivered on B leg of a bridge when A leg has no media #resolve
Applying: FS-9854: [mod_sofia] SDP O/A fails to put sdp in messages after certain kinds of sip traffic
Applying: FS-9876 switch_rtp this fix issue of rtcp lost packet count
Applying: FS-4102: [mod_sofia] invite to gateway without registration goes to another wrong host #resolve
Applying: FS-6893 [mod_conference] recording auto creates file path if not exists
Applying: FS-9840 mod_sofia: fix redefine warning
Applying: FS-9840 mod-verto: fix implicit declaration warning
Applying: FS-9840 sofia-sip: fix implicit declaration warning
Applying: FS-9840 mod_avmd: Fix implicit declaration warning
Applying: [verto-communicator] - Added change server feature
Applying: [mod_callcenter] FS-9723: Fixing cc_warning_tone, using switch_ivr_play_file instead of queue private event
Applying: [mod_callcenter] FS-9689 Fixed issue when agent rejects the call while hearing cc_outbound_announce making member (caller) waiting on queue forever
Applying: [mod_callcenter] FS-9347: Do not log as error when the member is gone just before we assigned an agent, now logging as DEBUG
Applying: [mod_callcenter] FS-9891: Get queue again to increase calls answered and abandoned
Applying: FS-9569: [mod_shout] close file handle when recording mp3 files that never get written to
Applying: FS-9776: [mod_sofia] SIP Transfer generates high CPU #resolve
Applying: [mod_callcenter] FS-9891: Checking if we got a valid pointer for a queue
Applying: [mod_callcenter] FS-9894: Removing reference to call_timeout, use leg_timeout instead
Applying: swigall
Applying: FS-9206 backport
Applying: swigall
Applying: FS-9898: [mod_sofia] Call hanging in FS if HOLD not successful  #resolve
Applying: FS-9897 [mod_v8] Fix Visual Studio 2015 build
Applying: FS-9860: [core] add playback_timeout_sec_cumulative variable
Applying: FS-9881: [core] FS crashing when playing png file #resolve
Applying: FS-9911: [mod_conference] Canvas not clearing when video playback of file is done #resolve
Applying: FS-9912: [mod_conference] floor-only and file-only not working properly in canvas-layouts #resolve
Applying: FS-9840: [mod_avmd] fix error avmd_desa2_tweaked.c:61:5: error: implicit declaration of function 'ISINF' [-Werror=implicit-function-declaration]
Applying: FS-9206: [mod_sofia] proxy media with enable-3pcc=proxy does not properly pass audio after 3pcc re-invite #resolve
Applying: FS-9881 accidentally removed -1 for forever support
Applying: FS-9917 [switch_rtp/core] Fix in do_flush to handle the current packet (if RFC2833) rather than discard it.
Applying: FS-9916: [mod_spandsp] OB fax calls go zombie  #resolve
Applying: FS-9906: [mod_conference] Member join/part in conference shows webcam briefly during slide transition #resolve
Applying: FS-9855: [mod_spandsp] Refused T38 reinvite on b-leg breaks T38 negotiation on a-leg when using T38 gateway mode #resolve
Applying: FS-9915: [mod_sofia] fix non null terminated parsed sip body being passed in when sending to sip messages in a row on tcp in a single packet
Applying: FS-9809: [mod_sofia] url encode caller id number before sticking it in the from header in case we have non url safe chars in the cid number in the caller profile
Applying: swigall
Applying: swigall
Applying: swigall
Applying: swigall
Applying: FS-9581 #resolve remove file from core lib project file that was added inadvertantly
Applying: FS-9924: Removed libvpx dependency from SPEC file.
Applying: FS-9929: [core,mod_spandsp] Assert in switch_frame_buffer_dup when receiving a fax using t.38 #resolve
Applying: FS-9932: [freeswitch-core] Error with group confirm feature combined with enterprise originate #resolve
Applying: FS-9931: [mod_sofia] don't send display updates to endpoints who don't have UPDATE in their Allow header
Applying: FS-9933: [freeswitch-core] Fallback from native file failure to alternate ext #resolve
Applying: FS-9934: [mod_redis] fix segfault on windows on close or connect failure
Applying: Version bump
Applying: FS-9943: [core] Default 488 handling for t.38 re-invite switches to udptl mode when it should not. #resolve
Applying: FS-9978: [mod_expr] mod_expr random seed function not working for Windows #resolve
Applying: FS-9154 tiny part belongs here too
Applying: FS-9970: [mod_sofia] don't detect nat in cases when the contact is in the acl, but the packet actually came from a proxy.  We need to check where we got the packet from as being a natted address instead of the contact in order to properly handle nat to our next hop
Applying: FS-9975: [mod_sofia] add contact params to request uri of outbound recovery reinvite for originally inbound calls
Applying: FS-9962: [mod_spandsp] Avaya IP Office IB FAX call T38 v0 failed  #resolve
Applying: FS-10003 small tweak to freeswitch.spec file for improper libilbc2 require %backport=1.6
Applying: FS-10008: [mod_say_en] Add military time to say_en #resolve
Applying: FS-10019: [mod_conference] Crash when playing mp4 in personal-canvas mode #resolve
Applying: FS-10021: [RTP] Large RTP timestamp jump when system clock is late from internal timer #resolve
Applying: FS-10022: [mod_sofia] verify-profile 'none' not parsed for TLS #resolve
Applying: FS-9944 [core] Add core video support to windows build
Applying: FS-9946 [verto_communicator] Fixed video size when window is resized on canary
Applying: FS-9947 [verto_communicator] Do not try to parse empty chat messages
Applying: FS-9948 [mod_png] Add mod_png to windows build
Applying: FS-9654 introduce origination_aleg_uuid
Applying: FS-9954: [freeswitch-core] Crash on switch_ivr_intercept_session due null pointer for buuid #resolve
Applying: FS-9955 [mod_kazoo] set profile var when setting channel var
Applying: FS-9959 [mod_spandsp] Add two new channel variables
Applying: FS-9981: [mod_spandsp] add api_on_fax_success api_on_fax_failure #resolve
Applying: FS-9989 [mod_kazoo] add a parameter to ignore transient network issues
Applying: FS-9986: [libvpx] update .gitignore to exclude erronesously matched Makefile
Applying: FS-9984 [mod_enum] Fix for handle leak in Windows
Applying: FS-9953 [mod_av] Add mod_av to windows build
Applying: [mod_sofia] FS-9966 fix private ip in contact header when invite w/ nosdp
Applying: FS-9997: [mod_verto] Invalid JSON-RPC Response to an incorrect JSON-RPC Request. #resolve
Applying: FS-6683: [mod_dingaling] iksemel TLS-Fragments #resolve
Applying: FS-10012 [mod_callcenter] Enabling bypass media if agents leg have bypass_media_after_bridge=true #resolve
Applying: FS-10017: [freeswitch-core] add rtp_nack_buffer_size #resolve
Applying: FS-10020: [mod_av] Error scrolls endlessly on a recording that fails to rtmp addrs #resolve
Applying: FS-10006: [core] Allow adding parameters to P-Asserted-Identity #resolve
Applying: FS-9137 update to openh264 release 1.5.0 and tweak some params
Applying: FS-9137 update to openh264 release 1.6.0
Applying: FS-10007: [mod_conference] Issue with reservation-id and conference video layouts #resolve
Applying: FS-9654 additional changes
Applying: FS-9990: [freeswitch-core] Exhaust fmtp sensitive codecs before moving on with negotiation in video #resolve
Applying: FS-9206: [core] endable proxy media auto-adjust on re-invite for text and video every time as the streams may be being added on re-invite
Applying: FS-10010 [WIX] Fix windows WIX installer by including a proper vc runtime
Applying: FS-10025: fix global symbol scope issue causing modules to use another modules global pointer
Applying: FS-10026: [mod_verto] reduce attach_wake calls #resolve
Applying: FS-10031: [mod_conference] Personal canvas mode doesn't switch layouts properly when a group is specified #resolve
Applying: FS-10034 [WIX] Fix WIX to respect Win32/x64 binary output folders.
Applying: bump version
Applying: FS-10035: [mod_sofia] Outbound calls use progressing not alerting #resolve
Applying: drop mod_ldap for meta-all
Applying: Revert "FS-10003 small tweak to freeswitch.spec file for improper libilbc2 require %backport=1.6"
Applying: FS-10003 #resolve force mod_ilbc package to depend on ilbc2 with Requires in spec file this builds properly as its linked against ilbc2 via the build requires
Applying: FS-10048: [mod_conference] Possible crash on mass exit of members from a conference #resolve
Applying: FS-10019 revert and alternate fix
Applying: FS-10054: [mod_smpp] mod_smpp will not reconnect if connection was interupted #resolve
Applying: FS-10054
Applying: FS-10055: Fix gentls_cert script to use "@certsdir@"
Applying: FS-10056: Fix modcheck.sh invokation
Applying: FS-10088: [freeswitch-core] Backports #resolve
Applying: FS-10088: [freeswitch-core] Backports
Applying: FS-10088: [freeswitch-core] Backports
Applying: FS-10091: [mod_conference] Conference play file with full-screen=true has side effect on member video #resolve
Applying: FS-10097: [mod_conference] Add fgimg to conference video layouts
Applying: FS-10037 [Core] Update OpenSSL to version 1.0.2k for windows
Applying: FS-10032: [mod_amqp] Fix log facilities #resolve
Applying: FS-10038: [core] tune heartbeat events interval
Applying: FS-10041: [mod_conference,mod_sofia] Invalid contact,<(null)>;isfocus, when hold call inside a conference room #resolve
Applying: FS-10058: [mod_voicemail] voicemail timestamp plays in military time #resolve
Applying: FS-7989: [shell-utils] Fix bugs in fixbug.pl
Applying: FS-10067: [mod_sofia] add update-refresher profile param and sip_update_refresher channel var to use update for session timers
Applying: FS-10001: [core] Fix Buffer overflow collecting digits
Applying: FS-10061 [mod_verto] now it sends custom variables on incoming call via verto
Applying: FS-9864: mod_opus : Added OPUS@16000 with 10, 20, 40, 60 ms ptime
Applying: FS-10079: [mod_conference] Possible lockup when sending many commands to conference at once #resolve
Applying: FS-10091: [mod_conference] Conference play file with full-screen=true has side effect on member video
Applying: FS-10076: [mod_av] File sync issues with different framerates #resolve
Applying: FS-10103: [freeswitch-core] segfault on new call #resolve
Applying: FS-10104: [mod_h323] fix build error caused by FS-10025
Applying: FS-10114: [mod_conference] Reduce image reads from disk for logo image
Applying: FS-10019
Applying: FS-10019 typo
Applying: FS-10116: [RTP] Crash when rtp_autofix_timing=true on video calls #resolve
Applying: FS-10118: [freeswitch-core] Race conditions from lack of error checking in switch_core_session_read_lock #resolve
Applying: FS-10107: [mod_conference] Reduce contention on layer floor changes #resolve
Applying: FS-10107: [mod_conference] Reduce contention on layer floor changes
Applying: FS-10091: [mod_conference] Conference play file with full-screen=true has side effect on member video
Applying: FS-10125 [new_module] clearmode passthru codec
Applying: FS-10107
Applying: FS-10107 doh
Applying: FS-10131: [freeswitch-core] Incorrect video decode flags in some places #resolve
Applying: FS-10132: [mod_conference] Memory leak playing video files in personal-canvas mode #resolve
Applying: FS-10125 small fix
Applying: FS-10088: [freeswitch-core] Backports
Applying: FS-10088: [freeswitch-core] Backports backport video modification functions to fix a seg in conference caused by backporting FS-10107
Applying: FS-10107 can of worms contd
Applying: FS-9704: [mod_av] Seeking video files with mod_av after it reaches the end does not work fix regression
Applying: FS-10091
Applying: FS-9697 remove auto toggle to old fir mode
Applying: FS-10143 #resolve small tweak to overlaps conf layout
Applying: FS-10144: [mod_conference] Minor issues with video mute in mod_conference [cherry-picked]
Applying: FS-9704 backport of 2de527564e6079b4ddd8659cda0064e3d8eaacb1 cherry pick was beeing funky
Applying: FS-10146: [mod_conference] Completely setup conference member before adding them to the conference #resolve
Applying: FS-10152: [mod_shout] seek from eof to 0 not working in mod_shout #resolve
Applying: FS-10139: [verto.js]  $.FSRTC.bestResSupported() returns true for any res #resolve
Applying: FS-10150: [freeswitch-core] Reduce writes to closed ssl sockets #resolve
Applying: FS-10169: [mod_local_stream] When using local stream commands FreeSWITCH locks up #resolve
Applying: FS-10156 [mod_http_cache] Return HTTP status code
Applying: FS-10149 [freeswitch-core] ZRTP encrypted calls drop on reinvite
Applying: Resolves FS-10071. Fixed newer (perl 5.22 and up) versions of perl from crashing, -e means evaluate the following string and it does not like emptystr.
Applying: FS-10087: fix for volume level per member of conference (volume level when playing file)
Applying: FS-10128 [mod_v8] This commit removes strlen() in favor of binary safe .length() function
Applying: FS-10180: [mod_sofia] Move debug line to higher level #resolve
Applying: FS-10183 [mod_sofia] Broadsoft Shared line pickup would fall if a-leg is PCMU and your pickup device has G722 as its first codec.
Applying: FS-10184: [Build-System] Fix missing #ifdefs for proper build w/o libyuv/libvpx
Applying: FS-10193: [freeswitch-core] Implement filter in openssl to enable proper dtls MTU #resolve
Applying: FS-10193: fix osx build error
Applying: version bump
Applying: FS-9765 one tweak from submitted patch to use switch_channel_var_true instead of switch_channel_get_variable no need to allocate on every hold/unhold just to check if this is enabled.
Applying: FS-10099: [mod_conference] fix rare seg on race on shutdown of a conference
Applying: FS-10126: [freeswitch-core] General Video Improvements
Applying: FS-10150: [freeswitch-core] Reduce writes to closed ssl sockets
Applying: FS-10126: [freeswitch-core] General Video Improvements
Applying: FS-10126: [freeswitch-core] General Video Improvements #resolve
Applying: FS-10100: [mod_av] fix crash on allocation error and other error cases opening a file
Applying: FS-10222: [freeswitch-core] add disable_audio_jb_during_passthru and disable_video_jb_during_passthru (cherry pick of aaa26c6 was messed up in git so ported manually)
Applying: FS-10195: [fs_cli]  Freeswitch intermittently segfaults  #resolve
Applying: FS-10209: [freeswitch-core,mod_av] Add auth params to file handles
Applying: FS-10117 [mod_rayo] allow duplicate rayo signal-type configs for call progress detector
Applying: FS-10059: [sofia-sip] handle re-invites during t.38 call
Applying: FS-10153: [build] fix mod_http_cache build on FreeBSD
Applying: FS-10220: [mod_conference] Conference channel parameters not working #resolve
Applying: FS-10210: [mod_console] add support for uuid config param and 'console uuid' api command
Applying: FS-10225: [mod_conference] Incorrect layout chosen when playing a file in a conference with a layout group #resolve
Applying: FS-10225: [mod_conference] Incorrect layout chosen when playing a file in a conference with a layout group -- Edge case with file-only slots
Applying: FS-10233: [mod_local_stream] mod_local_stream segfault trying to read a music file that is not open while playing a chime #resolve
Applying: FS-10225: [mod_conference] Incorrect layout chosen when playing a file in a conference with a layout group -- revert small piece
Applying: bump version
Applying: FS-10225
Applying: FS-9623: fix rare crash on startup due to openssl init functions being run multiple times
Applying: FS-10247: [mod_conference] Fit logo img to size of cropped video or mute image #resolve
Applying: FS-10241: [mod_sofia] don't send xml media refresh request before we have media setup
Applying: FS-10241 backport d157cbab12ee64d5577fee07d8ec78e82b7366ee manually
Applying: FS-10169: [mod_local_stream] When using local stream commands FreeSWITCH locks up #resolve
Applying: FS-10254: [mod_conference] Send keyframe from shared encoder on layout changes #resolve
Applying: FS-10255: [freeswitch-core] "complete" sqlite table grows indefinitely when video-mode=mux is enabled for conference #resolve
Applying: FS-10259: [freeswitch-core,mod_commands,mod_conference] Allow uuid_video_bitrate to supersede bitrate control from the conference
Applying: FS-10273: [freeswitch-core] Missing case stmt causing invalid stats #resolve
Applying: FS-10274: [mod_conference] Prevent double-recording of conference files and all recording of supercanvas in multi-canvas mode
Applying: FS-10286: [mod_conference] Sync member joins up with keyframes in shared encoder mode
Applying: FS-10284: [core] rtp session variable "ts" can wrap to zero for long running calls, causing incorrect logic to be executed #resolve
Applying: FS-10295: [freeswitch-core] Remove debug log line #resolve
Applying: FS-10307: [freeswitch-core] Repetitive verto re-attach with video only channels can cause a buffer overflow #resolve
Applying: FS-10311: [core] RTP timestamp rollover calculation is incorrect #resolve
Applying: FS-10311: [core] RTP timestamp rollover calculation is incorrect
Applying: FS-10320: [mod_av] Playing a file with audio only with mod_av can crash when attempting to seek #resolve
Applying: FS-10311: [core] RTP timestamp rollover calculation is incorrect
Applying: FS-10312: [mod_event_socket] bgapi uuid_transfer using -both option is not transfering both uuid's #resolve
Applying: FS-10312: [mod_event_socket] bgapi uuid_transfer using -both option is not transfering both uuid's
Applying: FS-10328: [freeswitch-core] Add method to allow orphaned B legs during originate to transfer to another extension #resolve
Applying: FS-10249: [mod_av] Audio gradually falls behind video in recordings #comment backport to 1.6
Applying: FS-10231 Fix issue with media bugs not being completely cleaned up when session is destroyed
Applying: FS-10335: [mod_av] Colors in recorded MP4 appear dull #resolve
Applying: FS-10231: [freeswitch-core] Some media bugs not fully cleaned up when session is destroyed #comment Regression causing deadlock when holding write lock and close/destroying all the bugs but the video recording thread tries to read lock.  Separating destroy out so it can be called outside the lock after the bugs are detached.
Applying: FS-10326: [mod_conference] Memory leak while playing video files that contain only a video stream #resolve
Applying: FS-10249: [mod_av] Audio gradually falls behind video in recordings
Applying: FS-10249: [mod_av] Audio gradually falls behind video in recordings
Applying: FS-10236: [core] fix crash on hangup with multiple media bugs
Applying: FS-10241: Convert sofia_send_info_vid_refresh to a chanvar.
Applying: FS-10251 [mod_rayo] fix defects found by clang-analyzer
Applying: FS-10169: [mod_local_stream] When using local stream commands FreeSWITCH locks up #resolve
Applying: FS-10169: [mod_local_stream] When using local stream commands FreeSWITCH locks up #resolve
Applying: FS-10246: [build] fix code can not be reached build error
Applying: FS-10225: [mod_conference] Incorrect layout chosen when playing a file in a conference with a layout group -- fix regression when playing files into a group layout
Applying: FS-10150: [freeswitch-core] Reduce writes to closed ssl sockets -- same fix for non-ssl sockets #resolve
Applying: FS-10258: [mod_sofia] FW must keep previously negotiated DTLS role during SIP re-INVITE
Applying: FS-10261: Fire conference-destroy event later
Applying: FS-10228: [switch_pgsql] Avoiding double openssl initialization when using core pgsql
Applying: FS-10264: extend switch_rtp_packet_t to fix jitter buffer bug triggered by RTP ext headers (RFC5285)
Applying: FS-10352 #resolve fix size doesn't match causing segs when casts to switch_rtp_packet_t
Applying: FS-10208: Exclude .git* files & gbp.conf from upstream tarball for Debian package.
Applying: FS-10268. Set correct Event-Name for RECV_EVENT event in sofia endpoint
Applying: FS-10270: [mod_conference] Regression in personal canvas -- from: f1d8685566bab20beabe82e27af6f895868d9d2f #resolve
Applying: FS-10246 [Core] Fix VPX codecs for windows build
Applying: [FS-10155] French digits are not spelled right
Applying: FS-10269: [mod_conference] conference recording pause doesn't work correctly for video -- partial
Applying: FS-10172: mod_event_socket: handle return codes from switch_queue_trypush() , more verbose logging when we cannot enqueue
Applying: FS-10111: mod_xml_cdr Create folder recursivery to the specified destination
Applying: FS-10085 [mod_callcenter] fix no_answer_delay_time behavoir in ring-all strategy
Applying: FS-10279: Fix double digit pronounciation (mod_say_nl).
Applying: FS-10282: mod_opus: fix some logging for debug mode (when opus_debug is on)
Applying: FS-10071 mod_perl safety fix for clone call
Applying: FS-9242: [verto.js] Update WebRTC code in verto to match latest spec -- update to latest
Applying: FS-10285: [verto.js] Device enumeration in Edge #resolve
Applying: FS-10291: [fs_cli] fs_cli Error indicated on console loglevel debug with extra whitespace before or after debug #resolve
Applying: FS-10267: [freeswitch-core] zrtp_enrollment broken since 1.6.13 #resolve
Applying: FS-10298 [mod_callcenter] Firing bridge-agent-end if we failed to bridge answered agent with member.
Applying: FS-10299 [mod_callcenter] Removing global lock on all cc_execute_sql functions when executing database queries
Applying: FS-10300: [mod_verto] fix crash in multiple verto messages when processing messages with missing params
Applying: FS-8941: [verto_communicator] Add No Microphone label to audio devices
Applying: FS-10310 [verto_communicator]: Adding validation at change login information modal
Applying: FS-10309: [verto_communicator] Add a loader that shows up when check network is called and vanishes when the request is completed.
Applying: FS-10328: [freeswitch-core] Add method to allow orphaned B legs during originate to transfer to another extension
Applying: FS-10338: [mod_sofia] add sip_invite_stamp variable of the time we received initial invite on an inbound call leg
Applying: FS-10319: fix build errors from rtp ts changes
Applying: FS-10269: [mod_conference] conference recording pause doesn't work correctly for video #resolve
Applying: FS-10249: [mod_av] Audio gradually falls behind video in recordings #comment Based on that feedback, please try latest master
Applying: FS-10269: [mod_conference] conference recording pause doesn't work correctly for video -- fix regression from change re: FS-10249
Applying: swigall
Applying: swigall
Applying: FS-10360: [freeswitch-core,verto.js] FireFox Screen Sharing #resolve
Applying: FS-10363 [Core] Move openssl to props on windows.
Applying: FS-10364 [mod_event_multicast] Enable encryption on windows by adding missing defines.
Applying: FS-10365 [mod_http_cache] Add mod_http_cache to the windows build.
Applying: FS-10362 [mod_lua] Update lua to 5.2.4 for windows build.
Applying: FS-10369: [freeswitch-core] Preserve original progress time when getting more than one #resolve
Applying: update phrase
Applying: FS-10189: [core] switch_core_add_state_handler runtime.state_handler_index may exceed SWITCH_MAX_STATE_HANDLERS  #resolve
Applying: FS-10257: [mod_sofia] libsofia-sip-ua/msg no longer builds on Arch Linux due to -Werror=parentheses
Applying: FS-10240: [freeswitch-core] Use the "Negative Lookahead" in xml dialplan cause memory leak #resolve
Applying: FS-10369: [freeswitch-core] Preserve original progress time when getting more than one
Applying: FS-10371: [mod_httapi] Typo in httapi causes files to always report video #resolve
Applying: FS-10372: [Build-System,fs-utils]  #resolve
Applying: FS-10084 [mod_v8] If the value passed is negative, block until event is received
Applying: FS-10378: [freeswitch-core] VPX Tweaks #resolve
Applying: FS-10379: [mod_conference] Set canvas size based on a variable
Applying: FS-10378: [freeswitch-core] VPX Tweaks
Applying: FS-10383 [freeswitch-core] Destroy RTP session write timer
Applying: version bump
Applying: manually cherry pick 7c19615 -- FS-10417: [freeswitch-core] Reduce flicker in screen sharing
Applying: FS-10405: [core] Fix Timer destroy error on one legged calls
Applying: FS-10387: [core] High memory usage with mod_sofia, osmo-nitb and DTX setting active #resolve
Applying: FS-9717 [Build-System] Fix missing mods in msi packages. Fix x64 release build on a clean tree on windows.
Applying: FS-10417: [freeswitch-core] Reduce flicker in screen sharing -- minor tweak
Applying: FS-10405: fix typo in timer destroy check
Applying: FS-10409: [core] Crash (ABRT) using conferencing -- related to FS-10132 #resolve
Applying: FS-10249: [mod_av] Audio gradually falls behind video in recordings -- init timer for audio only as well
Applying: FS-10394: [freeswitch-core] FS Crash while linphone sends ICE packets #resolve
Applying: FS-10394: [freeswitch-core] FS Crash while linphone sends ICE packets
Applying: FS-10433: [mod_av,mod_conference] Crash when video recording fails to setup properly #resolve
Applying: hand pick db477925584cb67b14c4314bc8a8a1bc4aa5a959 - FS-10447: [freeswitch-core] Manual video refresh mode
Applying: FS-10433: [mod_av,mod_conference] Crash when video recording fails to setup properly -- fix regression
Applying: hand pick 8734c9070db99be60ac251dab65a6d6440af2719 FS-10448: [mod_conference] Add Video Blind
Applying: FS-10440: [mod_httapi] valgrind: event leak in mod_httapi.c #resolve
Applying: FS-9785: fix ./mod_conference.h:353:23: error: enumerator value for ‘EFLAG_BLIND_MEMBER’ is not an integer constant expression [-Werror=pedantic] EFLAG_BLIND_MEMBER = (1 << 31)
Applying: FS-10448: [mod_conference] Add Video Blind -- add tweak
Applying: FS-10454: [mod_av] Regression in video file seek #resolve
Applying: FS-10456: [mod_av] add wav support to mod_av as well as specifying audio_codec #resolve
Applying: FS-10456: [mod_av] add wav support to mod_av as well as specifying audio_codec -- add av_record_audio_only param
Applying: FS-10286: [mod_conference] Sync member joins up with keyframes in shared encoder mode -- high cpu usage on h264
Applying: FS-10472: [mod_conference] Invalid free in personal canvas mode #resolve
Applying: FS-10473: [freeswitch-core] FreeSWITCH crash - Null event pointer dereference during conference_cdr_del #resolve
Applying: FS-10448: [mod_conference] Add Video Blind -- make blind video feature work in passthrough mode too
Applying: FS-10472: [mod_conference] Invalid free in personal canvas mode
Applying: FS-10472: [mod_conference] Invalid free in personal canvas mode - manual cherry-pick of 2ee8d58
Applying: FS-10384 [mod_lua] Fix Makefile target
Applying: FS-10370: Enable SRTP Key Padding
Applying: FS-10356: [core] Do not blindly print error string from rtp/stun packets
Applying: FS-10406: [mod_sofia] mod_sofia secure websocket connections SSLv3 and tls v1.0 is still not disabled  #resolve
Applying: FS-9894 [verto_communicator] Removing refreshDevices before doing a call.
Applying: FS-10427: move libesl to use swig3.0 so we can reswig on debian9
Applying: FS-10427: move mod_java to use swig3.0 so we can reswig on debian9
Applying: FS-10427: move mod_perl to use swig3.0 so we can reswig on debian9
Applying: FS-10427: move mod_python to use swig3.0 so we can reswig on debian9
Applying: FS-9785: fix build deps for mod_java
Applying: FS-10431: [mod_smpp] fix build on newer compilers due to malformed system headers
Applying: FS-10432 [mod_callcenter] Increase agent contact field up to 1024.
Applying: FS-10444 [vanilla config/languages] Adding phrases and macros tag to languages es, pt and sv
Applying: FS-10439: [mod_sofia] fix small leak when receiving REFER message
Applying: FS-9785: Fix format-truncation warnings for systems that treat it as an error.
Applying: FS-9785: Fix src/switch_ivr_play_say.c:1668:48: error: ‘*’ in boolean context, suggest ‘&&’ instead [-Werror=int-in-bool-context]
Applying: FS-10453 [kazoo] fix dropped messages
Applying: FS-10457: [mod_cdr_csv] set group too when creating new csv file so other users in the group can access it
Applying: FS-10458: [mod_av] temporarily silence warning when building against ffmpeg 3.2 until we fix them properly
Applying: FS-10466: [freeswitch-core] Add session to some log lines #resolve
Applying: FS-10304: [mod_callcenter] Prevent infinite logging when a stale queue member in found in the database
Applying: FS-10388: [core] fix crash on shutdown when using multiple meida bugs
Applying: FS-10388: [core] fix crash on shutdown when using multiple meida bugs
Applying: FS-10451: Updated sound files descriptions
Applying: Revert "FS-10299 [mod_callcenter] Removing global lock on all cc_execute_sql functions when executing database queries"
Applying: FS-10407: [mod_sofia] Set redirect variables when outbound_redirect_fatal is true
Applying: FS-10395: [mod_sofia] Fix ssl error handling in tls sip traffic
Applying: FS-10480: [mod_av] fix crash recording an audio only stream to an rtmp stream
Applying: FS-10427: move mod_lua to use swig3.0 so we can reswig on debian9
Applying: Revert "FS-10304: [mod_callcenter] Prevent infinite logging when a stale queue member in found in the database"
Applying: swigall
Applying: version bump
Applying: FS-10472: [mod_conference] Crash due to hangup race in conference personal canvas mode -- manual cherry pick
Applying: FS-10379: [mod_conference] Set canvas size based on a variable
Applying: FS-10472: [mod_conference] Crash due to hangup race in conference personal canvas mode -- Regression fixed with playing files cont (manual cherry-pick)
Applying: FS-10472: [mod_conference] Crash due to hangup race in conference personal canvas mode -- the saga continues
Applying: FS-10472: [mod_conference] Crash due to hangup race in conference personal canvas mode -- the saga continues
Applying: FS-10472: [mod_conference] Crash due to hangup race in conference personal canvas mode -- the saga continues
Applying: FS-10523: [freeswitch-core] Websocket disconnects prematurely  #resolve
Applying: FS-10091: [mod_conference] Conference play file with full-screen=true has side effect on member video -- - manual cherry pick
Applying: FS-10526: [freeswitch-core] Uninitialized variable in switch_img_fit when using SWITCH_FIT_SIZE_AND_SCALE #resolve
Applying: FS-10527: [mod_av] AV tweaks
Applying: FS-10528: [mod_conference] Put proper color behind letterboxed video avatars #resolve
Applying: FS-10532: [mod_av] Add an av command to mod_av and use it to modify log level #resolve
Applying: FS-10379: [mod_conference] Set canvas size based on a variable -- lock width and height to even numbers
Applying: FS-10472: [mod_conference] Crash due to hangup race in conference personal canvas mode -- minor regression
Applying: FS-10562: [core] Crashes referencing cannot access memory #comment Firefox sending only candidates for RTCP and not RTP causing funky code path #resolve
Applying: FS-10577: [core] start additional event dispatch threads based on event system queue size
Applying: FS-10571: [mod_conference,RTP] TMMBR messages request the same size for any user layout size when manage-inbound-video-bitrate enabled
Applying: FS-10601: [freeswitch-core] accomodate should be accommodate #resolve
Applying: FS-10270 add additional patch
Applying: FS-10609: [mod_verto] Invalid pointer in verto channel #resolve
Applying: [core] FS-10587 502 response sent on codec mismatch
Applying: FS-10604: [core] Segfault in libcrypto / dtls #resolve
Applying: FS-10647: [mod_av] Video quality degragation from 1.6.17 to 1.6.19 #resolve
Applying: FS-10527: [mod_av] AV tweaks -- using more threads on decode is a little buggy
Applying: FS-10667: [core] Segfault in crypto / srtp #resolve
Applying: FS-10667: [core] Segfault in crypto / srtp #resolve
Applying: FS-10574: fix deadlock on invalid syntax using conference record api
Applying: FS-10527: [mod_av] AV tweaks
Applying: FS-10734: [mod_conference] Fix deadlock on hangup race #resolve
Applying: FS-10667: [core] Segfault in crypto / srtp
Applying: FS-10757: [mod_conference] Race condition freeing avatar image #resolve
Applying: cleanup unused
Applying: version bump
Applying: FS-11061: [core] fix build with newer pcre
Auto packing the repository in background for optimum performance.
See "git help gc" for manual housekeeping.
Enumerating objects: 43880, done.
Counting objects: 100% (14432/14432), done.
Delta compression using up to 12 threads
Compressing objects: 100% (14389/14389), done.
Writing objects: 100% (14432/14432), done.
Total 14432 (delta 10307), reused 0 (delta 0)
Removing duplicate objects: 100% (256/256), done.

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git branch
* (HEAD detached from refs/heads/v1.6)
  master
  v1.6
  v1.6_new

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git log
commit b0c0255facd0ea0f833f93e4a69c996ccb6cb305 (HEAD)
Author: Mike Jerris <mike@jerris.com>
Date:   Thu Mar 29 11:17:44 2018 -0400

    FS-11061: [core] fix build with newer pcre

commit 09a8f8d2dc0a14288c36bae854c0edc44be966e5
Author: Mike Jerris <mike@jerris.com>
Date:   Tue Jan 23 15:49:09 2018 -0600

    version bump

commit c7b3055c84eb20281e9b7ea9b436b5edae46b610
Author: Mike Jerris <mike@jerris.com>
Date:   Mon Nov 20 11:30:50 2017 -0500

    cleanup unused

commit c1c8327e78f7873cade756c4bad1543a91ba2e25
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Wed Oct 25 14:20:45 2017 -0500

    FS-10757: [mod_conference] Race condition freeing avatar image #resolve

commit 56c00ae70c22b99efffa11e6a24b5f3663b2b012
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Tue Oct 24 14:01:30 2017 -0500

    FS-10667: [core] Segfault in crypto / srtp

commit 4eb0c144ae300b18a5a29a6084bbd96a76b0cebf
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Thu Oct 19 13:24:40 2017 -0500

    FS-10734: [mod_conference] Fix deadlock on hangup race #resolve

commit 2df48d83b4922c8209f17df06bb6fa4db2f5025b
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Wed Sep 27 12:58:04 2017 -0500

    FS-10527: [mod_av] AV tweaks

commit 4c2d352d561d3836782583a4f5f69ca1874d9988
Author: Mike Jerris <mike@jerris.com>
Date:   Sat Aug 5 13:11:59 2017 -0500

    FS-10574: fix deadlock on invalid syntax using conference record api

commit 1daad323dc3121aae13f3149ee980132c327fcb7
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Thu Sep 14 18:09:35 2017 -0500

    FS-10667: [core] Segfault in crypto / srtp #resolve

commit 0d5a92c23ff18cdcf240cc306e0517aca7595244
Author: Anthony Minessale <anthm@freeswitch.org>

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git branch
* (HEAD detached from refs/heads/v1.6)
  master
  v1.6
  v1.6_new

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git checkout v1.6_new
Checking out files: 100% (2116/2116), done.
Warning: you are leaving 1706 commits behind, not connected to
any of your branches:

  b0c0255fac FS-11061: [core] fix build with newer pcre
  09a8f8d2dc version bump
  c7b3055c84 cleanup unused
  c1c8327e78 FS-10757: [mod_conference] Race condition freeing avatar image #resolve
 ... and 1702 more.

If you want to keep them by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> b0c0255fac

Switched to branch 'v1.6_new'

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git reset --hard b0c0255facd0ea0f833f93e4a69c996ccb6cb305
Checking out files: 100% (2116/2116), done.
HEAD is now at b0c0255fac FS-11061: [core] fix build with newer pcre

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git log
commit b0c0255facd0ea0f833f93e4a69c996ccb6cb305 (HEAD -> v1.6_new)
Author: Mike Jerris <mike@jerris.com>
Date:   Thu Mar 29 11:17:44 2018 -0400

    FS-11061: [core] fix build with newer pcre

commit 09a8f8d2dc0a14288c36bae854c0edc44be966e5
Author: Mike Jerris <mike@jerris.com>
Date:   Tue Jan 23 15:49:09 2018 -0600

    version bump

commit c7b3055c84eb20281e9b7ea9b436b5edae46b610
Author: Mike Jerris <mike@jerris.com>
Date:   Mon Nov 20 11:30:50 2017 -0500

    cleanup unused

commit c1c8327e78f7873cade756c4bad1543a91ba2e25
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Wed Oct 25 14:20:45 2017 -0500

    FS-10757: [mod_conference] Race condition freeing avatar image #resolve

commit 56c00ae70c22b99efffa11e6a24b5f3663b2b012
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Tue Oct 24 14:01:30 2017 -0500

    FS-10667: [core] Segfault in crypto / srtp

commit 4eb0c144ae300b18a5a29a6084bbd96a76b0cebf
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Thu Oct 19 13:24:40 2017 -0500

    FS-10734: [mod_conference] Fix deadlock on hangup race #resolve

commit 2df48d83b4922c8209f17df06bb6fa4db2f5025b
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Wed Sep 27 12:58:04 2017 -0500

    FS-10527: [mod_av] AV tweaks

commit 4c2d352d561d3836782583a4f5f69ca1874d9988
Author: Mike Jerris <mike@jerris.com>
Date:   Sat Aug 5 13:11:59 2017 -0500

    FS-10574: fix deadlock on invalid syntax using conference record api

commit 1daad323dc3121aae13f3149ee980132c327fcb7
Author: Anthony Minessale <anthm@freeswitch.org>
Date:   Thu Sep 14 18:09:35 2017 -0500

    FS-10667: [core] Segfault in crypto / srtp #resolve

commit 0d5a92c23ff18cdcf240cc306e0517aca7595244
Author: Anthony Minessale <anthm@freeswitch.org>

A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git branch
  master
  v1.6
* v1.6_new

A@DESKTOP-84IQ1ND D:\work\freeswitch2
> git status
On branch v1.6_new
nothing to commit, working tree clean

A@DESKTOP-84IQ1ND D:\work\freeswitch2
>
A@DESKTOP-84IQ1ND D:\work\freeswitch2
```
