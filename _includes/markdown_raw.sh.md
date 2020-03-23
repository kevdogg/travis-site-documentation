```bash
#! /bin/bash

FILES=('*' '.*')

NEWDIRECTORY="/Users/kevdog/git/travis-Site-Documentation/_includes/"

for i in ${FILES[@]}; do
for file in $i; do
  echo File: $file
  filename=$(basename -- $file)
  extension="${filename##*.}"
  filename="${filename%.*}"

  if [ "${file}" != "." ] && [ "${file}" != ".." ]; then
    echo filename:${filename}
    echo extension:${extension}
    NEWPATH="${NEWDIRECTORY}${file}.md"
    echo ${NEWPATH}

    # Reference Command Line:
    #  ex -sc '1i|```bash' -sc '.' -sc '$' -sc 'a|```' -sc 'wq! new_file' ssl-modern-params.conf.txt
    echo "Processing $file -> Redirect to: ${NEWPATH}"
    echo "$ (ex -sc '1i|```bash' -sc '.' -sc '$' -sc 'a|```' -sc 'wq! ${NEWPATH}' ${file})"
    $(ex -sc '1i|```bash' -sc "wq! ${NEWPATH}" ./${file})
    $(echo '```' >> ${NEWPATH})

  fi

done
done


```
