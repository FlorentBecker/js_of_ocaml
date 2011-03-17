
NAME=toplevel
OBJS=toplevel.cmo

all: $(NAME).js

include ../Makefile.conf
COMP=../compiler/$(COMPILER)
JSFILES=../runtime/runtime.js ../runtime/weak.js toplevel_runtime.js
OCAMLC=ocamlfind ocamlc -package lwt,str -pp "camlp4o ../lib/syntax/pa_js.cmo" -I ../lib -I ../compiler
STDLIB= ../lib/$(LIBNAME).cma toplevellib.cma ../compiler/compiler.cma

# Removed gc and sys
STDLIB_MODULES=\
  arg \
  array \
  arrayLabels \
  buffer \
  callback \
  camlinternalLazy \
  camlinternalMod \
  camlinternalOO \
  char \
  complex \
  digest \
  filename \
  format \
  genlex \
  hashtbl \
  int32 \
  int64 \
  lazy \
  lexing \
  list \
  listLabels \
  map \
  marshal \
  moreLabels \
  nativeint \
  obj \
  oo \
  parsing \
  pervasives \
  printexc \
  printf \
  queue \
  random \
  scanf \
  set \
  sort \
  stack \
  stdLabels \
  stream \
  string \
  stringLabels \
  weak
PERVASIVES=$(STDLIB_MODULES) outcometree topdirs toploop

#toplevel.byte: $(OBJS:cmx=cmo) toplevel.cmo
#	ocamlfind ocamlc -linkall -g -package str -linkpkg toplevellib.cma -o $@.tmp $^

$(NAME).js: $(NAME).byte $(COMP) $(JSFILES)
	$(COMP) -toplevel -noinline -noruntime $(JSFILES) $(NAME).byte $(OPTIONS)

$(NAME).byte: $(OBJS) ../compiler/compiler.cma
	$(OCAMLC) -linkall -package str -linkpkg -o $@.tmp $(STDLIB) $^
	/usr/lib/ocaml/expunge $@.tmp $@ $(PERVASIVES)
	rm -f $@.tmp

%.cmo: %.ml
	$(OCAMLC) -c $<

%.cmi: ../compiler/compiler.cma

../compiler/compiler.cma:
	$(MAKE) -C ../compiler compiler.cma

clean::
	rm -f *.cm[io] $(NAME).byte $(NAME).js

depend:
	ocamldep -pp "camlp4o ../lib/syntax/pa_js.cmo" -I ../compiler *.ml *.mli > .depend

include .depend