%.html : %.txt ;
	asciidoc -b xhtml11 $^

%.gz : %.txt ;
	#a2x -f manpage $^ 2>&1 |grep -v '^Note: ' >&2
	#gzip -f --best $(patsubst %.txt,%, $^)
	asciidoc -d manpage -b docbook $^
	xsltproc --novalid --nonet /usr/share/xml/docbook/stylesheet/nwalsh/manpages/docbook.xsl  $(patsubst %.txt,%.xml, $^)
	# try to get rid of ".sp" sequences, see http://bts.grml.org/grml/issue567
	sed -i 's/\.sp//' $(patsubst %.txt,%, $^)
	gzip -f --best $(patsubst %.txt,%, $^)

MANPAGES = $(patsubst %.txt,%,$(wildcard *.txt))

all: doc

doc: doc_man doc_html
doc_html: $(addsuffix .html, $(MANPAGES))
doc_man: $(addsuffix .gz, $(MANPAGES))

clean:
	@for i in $(MANPAGES); do \
		rm -f $$i.html $$i.xml $$i.gz; done

