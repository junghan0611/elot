
# Table of Contents

1.  [Purpose](#orgcd77c70)
2.  [Directory layout](#org92e9905)
3.  [Provenance drawer (citation policy)](#org8e2c45d)
4.  [Planned fixtures](#org4f215b0)
5.  [Local numbering schemes](#org6e6216d)
6.  [Notes](#orge4e90a7)



<a id="orgcd77c70"></a>

# Purpose

This tree holds *small, deliberately-faulty (and a few deliberately-
clean) ontologies*, each citing a published authority for *why* it
is a known smell.  The fixtures are **test material**: things you
point an ELOT checker at to see whether it correctly fires (or
correctly stays silent).

Fixtures are grouped by the *authority* that named the pitfall, not
by which tool checks them.  A single OOPS! P24 fixture may be
exercised by:

-   the pure-Elisp lint sliver `elot/oops-recursive-definition`
    (Milestone 8, sliver), and/or
-   the SPARQL-backed `elot-oops-run` (Milestone 8, full suite,
    planned), and/or
-   ROBOT `verify` (Milestone 4) when wrapped as a query.

The *checkers themselves* live under `elot-package/` (Elisp) or
under `elot-package/elot-oops/queries/` (SPARQL, when that module
lands).  Do **not** store query files here.


<a id="org92e9905"></a>

# Directory layout

    test/fixtures/corpus/
    +-- README.org                  ; this file
    +-- oops/                       ; OOPS! pitfall catalogue
    |   +-- P32-duplicate-labels.org
    |   +-- ...
    +-- rector/                     ; OWL Pizzas / common pitfalls (Rector et al.)
    |   +-- R01-only-without-some.org
    |   +-- R02-domain-range-intersection.org
    |   +-- R03-missing-disjointness.org
    |   +-- R04-open-world-no-closure.org
    |   +-- R05-primitive-vs-defined.org
    |   +-- R06-domain-as-constraint.org
    +-- robot-report/               ; ROBOT report query coverage
    |   +-- clean-baseline.org      ; empty report expected
    |   +-- broken-multi.org        ; trips several report queries at once
    +-- reasoning/                  ; inconsistency / unsatisfiability cases
    +-- profile/                    ; OWL 2 DL profile violations
    +-- clean/                      ; deliberately-clean controls
        +-- minimal-ok.org

Each sub-tree may itself grow further structure later (e.g.
`oops/P11/` for multiple fixtures of the same pitfall).  The
meta-test (`test/elot-corpus-test.el`) walks the tree recursively.


<a id="org8e2c45d"></a>

# Provenance drawer (citation policy)

Every fixture file **must** begin with a property drawer of this shape:

    :PROPERTIES:
    :corpus-source:    OOPS!
    :corpus-id:        P32
    :corpus-severity:  Minor
    :corpus-url:       https://oops.linkeddata.es/catalogue.jsp#P32
    :corpus-expects:   elot/oops-duplicate-labels
    :END:

Field reference:

-   `:corpus-source:` - short name of the authority (`OOPS!`, `Rector`,
    `ROBOT-report`, `OBO`, `OWL2-Profile`, `OntoClean`, `clean`).
-   `:corpus-id:` - the authority's own identifier for the pitfall
    (`P32`, `OP1`, `annotation_whitespace`, &#x2026;).
-   `:corpus-severity:` - the authority's severity label
    (e.g. OOPS! uses `Critical` / `Important` / `Minor`).  Free
    text; not used for control flow.
-   `:corpus-url:` - canonical URL for the cited pitfall.
-   `:corpus-expects:` - what ELOT is expected to detect.  Three
    accepted shapes (see `elot-corpus-support.el`):
    
    -   an Elisp *checker symbol* like `elot/oops-duplicate-labels`
        (validated against `org-lint--checkers`);
    -   a *pitfall id* like `oops/P32` (validated against
        `elot-corpus--known-pitfalls`, a registry the OOPS!/ROBOT
        backends populate as they come online);
    -   a *planned slug* like `planned/rector-only-without-some`
        for fixtures whose checker is not yet implemented.  Such
        fixtures pass the meta-test but are skipped by
        per-backend end-to-end harnesses.
    
    For clean controls use `:corpus-expects: none`.
-   `:corpus-citation:` (optional) - free-text bibliographic
    citation (paper, section, figure) for prose sources like the
    Rector / OWL Pizzas tutorials, where `:corpus-url:` alone
    (e.g. a DOI) under-specifies the reference.
-   `:corpus-rationale:` (optional) - one short sentence explaining
    what the file demonstrates.

ERT failure messages include the citation automatically via
`elot-corpus-cite`.


<a id="org4f215b0"></a>

# Planned fixtures

The table below tracks *intended* corpus coverage.  Not all rows
exist yet; the meta-test only walks files actually present.  Step
8.1 ships the first two rows (and the clean control) so the
meta-test exercises a non-empty list from first commit.

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Sub-tree</th>
<th scope="col" class="org-left">File</th>
<th scope="col" class="org-left">Source</th>
<th scope="col" class="org-left">ID</th>
<th scope="col" class="org-left">Expects</th>
<th scope="col" class="org-left">Status</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">oops/</td>
<td class="org-left">P32-duplicate-labels.org</td>
<td class="org-left">OOPS!</td>
<td class="org-left">P32</td>
<td class="org-left">elot/oops-duplicate-labels</td>
<td class="org-left">present</td>
</tr>


<tr>
<td class="org-left">oops/</td>
<td class="org-left">P24-recursive-definition.org</td>
<td class="org-left">OOPS!</td>
<td class="org-left">P24</td>
<td class="org-left">elot/oops-recursive-definition</td>
<td class="org-left">present</td>
</tr>


<tr>
<td class="org-left">oops/</td>
<td class="org-left">P08-missing-annotations.org</td>
<td class="org-left">OOPS!</td>
<td class="org-left">P08</td>
<td class="org-left">elot/oops-missing-annotations</td>
<td class="org-left">present</td>
</tr>


<tr>
<td class="org-left">oops/</td>
<td class="org-left">P10-missing-disjointness.org</td>
<td class="org-left">OOPS!</td>
<td class="org-left">P10</td>
<td class="org-left">oops/P10</td>
<td class="org-left">planned</td>
</tr>


<tr>
<td class="org-left">oops/</td>
<td class="org-left">P11-missing-domain-range.org</td>
<td class="org-left">OOPS!</td>
<td class="org-left">P11</td>
<td class="org-left">oops/P11</td>
<td class="org-left">planned</td>
</tr>


<tr>
<td class="org-left">rector/</td>
<td class="org-left">R01-only-without-some.org</td>
<td class="org-left">Rector</td>
<td class="org-left">R01</td>
<td class="org-left">planned/rector-only-without-some</td>
<td class="org-left">present</td>
</tr>


<tr>
<td class="org-left">rector/</td>
<td class="org-left">R02-domain-range-intersection.org</td>
<td class="org-left">Rector</td>
<td class="org-left">R02</td>
<td class="org-left">planned/rector-domain-range-intersection</td>
<td class="org-left">present</td>
</tr>


<tr>
<td class="org-left">rector/</td>
<td class="org-left">R03-missing-disjointness.org</td>
<td class="org-left">Rector</td>
<td class="org-left">R03</td>
<td class="org-left">planned/rector-missing-disjointness</td>
<td class="org-left">present</td>
</tr>


<tr>
<td class="org-left">rector/</td>
<td class="org-left">R04-open-world-no-closure.org</td>
<td class="org-left">Rector</td>
<td class="org-left">R04</td>
<td class="org-left">planned/rector-missing-closure</td>
<td class="org-left">present</td>
</tr>


<tr>
<td class="org-left">rector/</td>
<td class="org-left">R05-primitive-vs-defined.org</td>
<td class="org-left">Rector</td>
<td class="org-left">R05</td>
<td class="org-left">planned/rector-primitive-vs-defined</td>
<td class="org-left">present</td>
</tr>


<tr>
<td class="org-left">rector/</td>
<td class="org-left">R06-domain-as-constraint.org</td>
<td class="org-left">Rector</td>
<td class="org-left">R06</td>
<td class="org-left">planned/rector-domain-as-constraint</td>
<td class="org-left">present</td>
</tr>


<tr>
<td class="org-left">robot-report/</td>
<td class="org-left">clean-baseline.org</td>
<td class="org-left">ROBOT-report</td>
<td class="org-left">-</td>
<td class="org-left">none</td>
<td class="org-left">present</td>
</tr>


<tr>
<td class="org-left">robot-report/</td>
<td class="org-left">broken-multi.org</td>
<td class="org-left">ROBOT-report</td>
<td class="org-left">-</td>
<td class="org-left">planned/robot-report-multi-violations</td>
<td class="org-left">present</td>
</tr>


<tr>
<td class="org-left">reasoning/</td>
<td class="org-left">disjoint-membership.omn</td>
<td class="org-left">reasoner</td>
<td class="org-left">C1</td>
<td class="org-left">elot_unsatisfiable</td>
<td class="org-left">planned</td>
</tr>


<tr>
<td class="org-left">profile/</td>
<td class="org-left">non-simple-asymmetric.org</td>
<td class="org-left">OWL2-DL</td>
<td class="org-left">P1</td>
<td class="org-left">(TBD)</td>
<td class="org-left">planned</td>
</tr>


<tr>
<td class="org-left">clean/</td>
<td class="org-left">minimal-ok.org</td>
<td class="org-left">clean</td>
<td class="org-left">-</td>
<td class="org-left">none</td>
<td class="org-left">present</td>
</tr>
</tbody>
</table>


<a id="org6e6216d"></a>

# Local numbering schemes

Where the cited authority publishes canonical pitfall ids
(OOPS! `P01..P41`, ROBOT-report query names), the corpus uses
them verbatim.  Where the authority is prose (Rector / OWL
Pizzas tutorials, OntoClean, reasoner-debugging notes), the
corpus uses a *local* id scheme so file names stay short and
table rows sort sensibly:

-   `rector/Rxx-slug.org` - Rector / OWL Pizzas pitfalls.  The
    `Rxx` ids are local to this corpus; the binding to the
    cited section/figure of the published source lives in the
    fixture's `:corpus-citation:` field.

New local-id schemes (`OCxx` for OntoClean, `RSxx` for reasoner
stretches, &#x2026;) should be added to this section as those
sub-trees come online.


<a id="orge4e90a7"></a>

# Notes

-   Fixtures must be self-contained and small: a reader should be
    able to see the smell at a glance.
-   Use the `ex:` prefix for fictitious resources; never hijack a
    real ontology IRI in a faulty fixture.
-   ASCII-only.  No diagrams.  Keep each file under ~60 lines.

