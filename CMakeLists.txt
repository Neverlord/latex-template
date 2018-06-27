cmake_minimum_required(VERSION 3.10)

project(caf-streaming NONE)

include(cmake/UseLATEX.cmake)

set(LATEX_USE_SYNCTEX yes)

set(sources
  sections/introduction.tex
)

set(bibfiles
  own.bib
  programming.bib
)

set(bibdir "$ENV{BIBINPUTS}")

# Copies fname from bibdir to bib/{fname}.bib
macro(get_bib fname)
  configure_file("${bibdir}/${fname}"
                 "${CMAKE_CURRENT_SOURCE_DIR}/bib/${fname}"
                 COPYONLY)
endmacro()

foreach(fname ${bibfiles})
  message(STATUS "get ${fname}")
  get_bib(${fname})
  list(APPEND target_bibfiles "bib/${fname}")
endforeach(fname)

add_latex_document(
  main.tex
  INPUTS ${sources}
  IMAGE_DIRS figures
  BIBFILES ${target_bibfiles}
  FORCE_PDF
  TARGET_NAME caf-streaming
)
