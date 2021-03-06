# Makefile
# This program is part of the source code released for the book
#  "Hands-On Linux System Programming"
#  (c) Kaiwan N Billimoria
#  Packt Publishers
#
# From: Ch 5
ALL := membugs membugs_dbg membugs_dbg_asan membugs_dbg_ub membugs_dbg_msan

CROSS_COMPILE=
  #arm-none-linux-gnueabi-
CC=${CROSS_COMPILE}gcc
CL=${CROSS_COMPILE}clang

CFLAGS=-Wall
CFLAGS_DBG=-g -ggdb -gdwarf-4 -O0 -Wall -Wextra
CFLAGS_DBG_ASAN=${CFLAGS_DBG} -fsanitize=address
CFLAGS_DBG_UB=${CFLAGS_DBG} -fsanitize=undefined
CFLAGS_DBG_MSAN=${CFLAGS_DBG} -fsanitize=memory

all: ${ALL}
CB_FILES := *.[ch]

common.o: ../common.c ../common.h
	${CC} ${CFLAGS} -c ../common.c -o common.o
	${CC} ${CFLAGS} -m32 -c ../common.c -o common32.o
common_dbg.o: ../common.c ../common.h
	${CC} ${CFLAGS_DBG} -c ../common.c -o common_dbg.o

membugs: membugs.o common.o
	${CL} ${CFLAGS} -o $@ $^
membugs_dbg.o: membugs.c
	${CL} ${CFLAGS_DBG} -c membugs.c -o membugs_dbg.o
	${CL} ${CFLAGS_DBG_ASAN} -c membugs.c -o membugs_dbg_asan.o
	${CL} ${CFLAGS_DBG_UB} -c membugs.c -o membugs_dbg_ub.o
	${CL} ${CFLAGS_DBG_MSAN} -c membugs.c -o membugs_dbg_msan.o
membugs_dbg: membugs_dbg.o common_dbg.o
	${CL} -o membugs_dbg membugs_dbg.o common_dbg.o
	${CL} ${CFLAGS_DBG_ASAN} -o membugs_dbg_asan membugs_dbg_asan.o common_dbg.o
	${CL} ${CFLAGS_DBG_UB} -o membugs_dbg_ub membugs_dbg_ub.o common_dbg.o
	${CL} ${CFLAGS_DBG_MSAN} -o membugs_dbg_msan membugs_dbg_msan.o common_dbg.o

# indent- "beautifies" C code into the "Linux kernel style".
# (cb = C Beautifier :) )
# Note! original source file(s) is overwritten, so we back it up.
cb: ${CB_FILES}
	mkdir bkp 2> /dev/null; cp -f ${CB_FILES} bkp/
	indent -linux ${CB_FILES}

clean:
	rm -vf ${ALL} core* vgcore* *.o *~
