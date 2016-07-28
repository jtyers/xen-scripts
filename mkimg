#!/bin/bash

# Given a file or directory, create an ext4 image file containing that file

# Usage: mkimg <src> <dst.img>
# where <src> is a file that should reside in the image, and dst.img is the name
# of the destination image file

set -eu

die() {
	echo $@ >&2
	exit 1	
}

[ $# -lt 2 ] && die "Usage: `basename $0` <src.tgz> <dst.img>"

SRC="$1"
DST="$2"

# we no longer care about overwriting
#[ -f "$DST" ] && die "$DST already exists"

[ ! -f "$SRC" ] && die "$SRC does not exist"

dd if=/dev/null of="$DST" bs=1M seek=5
mkfs.ext4 -F "$DST"

MOUNT_DIR=`mktemp -d`
mount -t ext4 -o loop "$DST" "$MOUNT_DIR"

cp "$SRC" "$MOUNT_DIR"/

umount "$MOUNT_DIR"
