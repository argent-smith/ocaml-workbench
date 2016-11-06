WHAT IS THIS?
=============

Makefile and Dockerfile for a Docker-mounted workbench. They are
self-documenting enough, I believe. Here are some highlights, though:

* Current arch for the project is armhf (a.k.a. armv7). You can change
  the mother image to a usual Ubuntu to make it for Intel platform.

* The locale and the timezone are mine. You can set your own.

* *NB:* ARMv7 is a slow-compiler so be patient making it on your
  favorite Raspberry Pi ;)

* OCaml version here is 4.02.3 since it's taken from the official
  Ubuntu packages repo. If you know how to make 4.03 please feel free
  to teach me ;)
