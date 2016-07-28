#!/bin/bash

# edittar - easily modify the contents of a tar.gz file
# Usage: edittar <file.tgz>

set -eu

die() {
	echo $@ >&2
	exit 1
}

FILENAME="$1"
FILENAME=`dirname ${FILENAME}`/`basename ${FILENAME}`

[ -f "$FILENAME" ] || die "File does not exist"

EXTRACT_DIR=`mktemp -d`

echo "Extracting ${FILENAME}..."
tar zxf "$FILENAME" -C $EXTRACT_DIR

OLDDIR=`pwd`
cd $EXTRACT_DIR
echo "Starting new shell in extracted TAR archive. Make the changes you need to. When you exit the shell, the TAR will be re-created with the contents of this directory."
$SHELL
cd $OLDDIR

# recreate tar archive here
FBN=`basename "$FILENAME"`
OLD_FILENAME=`mktemp "${FBN}.XXXXXX"`
echo "Saving old TAR archive at $OLD_FILENAME"
mv "$FILENAME" "$OLD_FILENAME"

echo "Creating new archive"
tar cfz "$FILENAME" -C "$EXTRACT_DIR" .

echo "Success! Removing old TAR archive"
rm "$OLD_FILENAME"

echo "Done"
