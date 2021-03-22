<!--
SPDX-FileCopyrightText: 2021 Kirill Elagin <https://kir.elagin.me>

SPDX-License-Identifier: CC-BY-SA-4.0
-->

<!--
Compile:

  pandoc --bibliography=./bib.bib proposal.md -o proposal.pdf

  (optionally: --pdf-engine=xelatex)
-->

---
title: Implementing coupled relational symbolic execution for DP
subtitle: A project proposal
author: Kirill Elagin
bibliography: bib.bib
link-citations: true
csl: ieee-with-url.csl
---

Differential privacy is a framework that provides rigorous mathematical data
security guarantees. It is an active area of theoretical research into
techniques for disclosing information while protecting an individual’s privacy.
However, it will only be applicable in practice if software developers
have access to tools to assist them in developing private algorithms by detecting
issues in their programs.

Multiple approaches have been proposed, including type systems, program logics,
and methods based on model checking. Regardless of the specific approach taken,
the ultimate goal is to formally verify that the code written by a
programmer not only implements a secure data access mechanism, but also
does not contain any bugs introduced accidentally during the development.

# Coupled relational symbolic execution

_Coupled relational symbolic execution (CRSE)_[@crse21] is a novel approach
combining previous work on relational symbolic execution[@rse19], probabilistic
relational reasoning[@prr12], and approximate probabilistic couplings[@approx16].

The paper describes a simple imperative programming language `PFOR` that can
be used for the development of differentially private algorithms. It then introduces
three additional languages:

* `RPFOR`, whose relational semantics allows for reasoning about pairs of
  simultaneous executions of a `PFOR` program on two different inputs;
* `SPFOR`, a symbolic variant of `PFOR` used for its symbolic execution;
* `SRPFOR`, which can be equivalently viewed as a relational extension
  of `SPFOR` or as a language for symbolic execution of `RPFOR`.

As a program is executed in a relational and symbolic way, each
evaluation of probabilistic primitives results in the introduction of
a constraint corresponding to the existence of an approximate probabilistic
coupling on the outputs. Then CRSE checks whether there is a coupling between
the two outputs, or whether there is a violation of the coupling.

For a violation in some cases it is able to produce a counterexample.
Finding counterexamples is hard in general, thus the authors devise a set
of heuristics which make this task more tractable.

The key result of the paper is that the proposed technique is sound
both for proving and refuting differential privacy.

# The project

The goal of this project is to implement a practical toolkit based on the
ideas presented in the CRSE paper and to use it to prove or refute the
differential privacy of some algorithms.

The hope is that it should be possible to implement it as an _embedded
domain-specific language (eDSL)_ in Haskell. Haskell is a common choice
of the host language for eDSLs due to its rich type system together
with an expressive and flexible syntax.

## Plan

The first step is to study the CRSE paper[@crse21] to build the required
degree of familiarity with the technique. This will likely require
following references on probabilistic coupling[@lindvall92] (and its
use in DP verification[@coupling17][@approx16]) and symbolic execution[@symbolic]
(in particular relational symbolic execution[@prr12][@rse19]).

The next step will be to implement this framework (languages, symbolic
interpretation for collecting constraints, and constraint analysis) in
Haskell.

Finally, if the Haskell framework happens to actually work, it will be possible
to implement some correct and incorrect algorithms in it. It seems reasonable
to start with the examples given in the paper itself – the Laplace mechanism
and “Above Threshold”[@sparsevec] – and then proceed to some other algorithms,
if time permits.


# References
