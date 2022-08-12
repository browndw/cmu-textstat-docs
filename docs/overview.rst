cmu.textstat
============

The **cmu.textstat** package is for use in the **36-468/668** course
(Special Topics in Statistics & Data Science) at Carnegie Mellon
University.

Installing cmu.textstat
-----------------------

Use devtools to install the package.

.. code:: r

   devtools::install_github("browndw/cmu.textstat")

Running cmu.textstat
--------------------

The package itself serves primarily as wrapper for 4 other packages,
which will install automatically when you install **cmu.textstat**:

1. mda.biber
2. ngramr.plus
3. quanteda.extras
4. vnc

The documentation also includes a description of **pseudobibeR**. That
package needs to be installed separately. It is included here as it is
one way to generate data for the **mda.biber** functions.

When you load the **cmu.textstat** library, those 4 other packages will
attach, giving you access to all of their functions.

.. code:: r

   library(cmu.textstat)

The main functions in the packages associated with **cmu.textstat** are
described in the table below. The functions are designed to facilitate
the analysis of textual data, assisting in the exploration of questions
related to language variation and change, language use, and language
structure.

Many of the functions (though not all) were written to support the
processes and procedures described by `Brezina in the required
textbook <https://www.cambridge.org/core/books/statistics-in-corpus-linguistics/4E530F86B328B2287681AD240796D2CF>`__,
and replicate many of `his web-based statistics
tools <http://corpora.lancs.ac.uk/stats/toolbox.php>`__ in an R
environment.

Many of the functions are designed to be used at the end of `a
processing
pipeline <https://programminghistorian.org/en/lessons/basic-text-processing-in-r>`__.
For our class, we will laregly rely on
`tidyverse <https://www.tidyverse.org/>`__ packages and
`quanteda <http://quanteda.io/>`__ for pre-processing, corpus creation,
and tokenization.

Functions
---------

.. toctree::
   :maxdepth: 2

   functions/functions.md
