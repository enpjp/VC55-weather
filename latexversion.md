---
title: 'Achieving Analytical Fluency With Complex Data [^1] '
author:
- Paul J Palmer
- Michael Henshaw
- Russell Lock
date: 'Received: date / Accepted: date'
output: pdf_document
abstract: |
  Scientific analysis is formally presented as a rigid process typically
  Experiment; Analysis, Evaluation; and Conclusions.
  comprising: Review; Theory; Research question; Methodology;
  We do, however, question whether established methods for managing and
  analysing data are still appropriate for "Big Data", by which we mean
  data that is too big to be conveniently manipulated by manually
  intensive methods.
  is to justify why there is a need for new analytical techniques, given
  that the existing ones still work; the second is to show how an
  abstract perception of data impacts the analytical process.
  This presents two questions which we seek to address here: the first
  An example of the motivation for change is illustrated by the "Grammar
  of Graphics" (GoG) paradigm. GoG uses combinations of transforms to
  generate every possible graphic and tabulation from data presented in
  a suitable state allowing for a radical change in the analytic
  workflow, while still preserving the goals of scientific analysis.
  By framing this problem as one of transforming *data state*, we can
  mathematically describe general properties allowing the creation of
  re-usable code templates for data preparation. Coupled with "literate
  programming" techniques we show that this approach enables
  analytically fluent analysis of complex data.
bibliography: library.bib
---

# Introduction {#sec:introduction}

Scientific analysis is formally presented as a rigid process typically
comprising: Review; Theory; Research question; Methodology; Experiment;
Analysis, Evaluation; and Conclusions. This is a powerful framework that
has stood the test of time which is the underlying format of countless
academic publications and there is no reason to think that this format
will change.

If we look deeper, we can start to separate the actual process of
scientific discovery from the way in which it is reported. The
documentation of research and analysis that has already been completed,
is very different to an exploration where the outcomes are as yet
unknown. Nobel Prize winner @Szent-Gyorgyi1972 discussed this issue in
1972, long before the age of computers and "Big Data"[^2] introduced
computational and data intensive research methods [@Szent-Gyorgyi1972].
He described the Apollonian as following a formally prescribed research
path while a Dionysian explores the unknown. However, research and
reporting are necessarily linked and one should be a clear and true
interpretation of the other.

We argue here that this presumption the analytical process should follow
the final method of presentation is a hindrance to the adoption of new
analytical techniques. The term "reproducible research" has been coined
to describe this linkage, without defining how it may be achieved
leading to discussion on the topic by @Stodden2014
[@Lewis2018a; @Leeper2014; @Gentleman2007; @Peng2015; @Drummond2018] and
others.

Two questions arise from these discussions which we seek to address in
the following sections: the first is to justify why there is a need for
new analytical techniques, given that the existing ones still work? The
second is: how can changing our abstract perception of data beneficially
impact the analytical process?

Both the need and benefit are illustrated by the "Grammar of Graphics"
(GoG) paradigm [@Wilkinson2010]. GoG uses combinations of transforms to
generate every possible graphic and tabulation from data that is
presented in a suitable state. Thus, a researcher planning to use GoG
will focus on saving research data in suitable state, so she can make
many dynamic visualisations as work progresses, rather than waiting for
the end of the data collation phase.

This contrasts with the typical approach where difficulties with data
intensive analysis are often retrospectively blamed on imperfections
within the data that are seen as requiring "cleaning" before analysis
can begin. It is unfortunate that the popular spreadsheet encourages a
manually intensive formatting of data and the use of visualisation tools
that do not scale well with data size
[@Mack2018; @Bishop2013; @Powell2008].

Our alternative approach is consider data as observations of the real
world that are to be interpreted through analysis, rather than formatted
tables meeting arbitrary technical specifications.[^3] This also expands
on an idea hinted at in the GoG example above: there is no need to
rigidly define data format in advance since the real world is clearly
independent of the methods we choose to observe and record. If we
believe that the knowledge we seek is encapsulated within a set of
observations, then we can choose a viewpoint where we see data as
starting in an initial *inconvenient state*. It follows that we must
transform it into a *convenient state* for analysis. By constructing a
working theory supported with a mathematical description, we show that
the transformation of *data state* can be generalised to allow the
creation of re-usable code templates. To explain how re-framing the
problem of *imperfect data* in terms of *data state* can make such a
difference to the way in which we approach the analytical process, we
must first construct a methodology that takes into account the nature of
data.

# Methodology {#sec:methodology}

The two questions we seek to answer are: "Why develop new analytical
techniques, given that the existing ones still work?" and "How can
changing our abstract perception of data beneficially impact the
analytical process?" To achieve this goal we follow a pragmatic
methodology that draws inspiration from the critical realism philosophy
of [@Bhaskar2008] which gives context to data as observations arising
from an unknowable real world. Our methodology is pragmatic in that we
use public domain data as the case study for each step in our analytical
approach.

We do not, however, try to justify the analytical *results* from our
case study, as the intent of this work is to demonstrate how *analytical
fluency* can be achieved when faced with the complexities of real data.
The reason for this caveat is due to the topical subject of the case
study and how it relates to the unknowable real world. First, we
complete our description of the methodology as a series of descriptive
steps.

Case Study:

:   The analytical process is demonstrated with data with the following
    characteristics:

    -   Open Source;

    -   Presented in an inconvenient state for analysis;

    -   Multiple units of measure;

    -   Extensible, in that related similar sources are available.

Data Definition:

:   Consider the philosophical nature of data with special reference to
    the case study to create a data model.

Apply the model:

:   The practical demonstration of data ready for analysis.

Template Concept:

:   Building reusable programmatic elements for analysis.

Template Evaluation:

:   A demonstration and evaluation of the template and its role in
    supporting analytical fluency.

Throughout this work we have based the practical analysis on the R
Analytical language [@RCoreTeam2017]. In principle, the techniques
presented here would work with other languages such as Python
[@Gentleman2007], but their potential has not been explored.

# Case Study {#sec:case-study}

The case study uses data from the Sutton Bonnington[^4] long term
weather monitoring station which is freely available online from
[@UKMetrologicalOffice2022]. As
Figure [1](#fig:suttonbonningtondata){reference-type="ref"
reference="fig:suttonbonningtondata"} shows, it contains observations
from 1959 to the present day, and is presented as a text file in an
inconvenient format for analysis. The files contain a series of weather
related observations in various units. To complicate matters some are
annotated and some are missing. Multiple similar source files available
for download, so the issues in this file are typical of a much larger
data set.


However, if we unpick the file shown in
Figure [1](#fig:suttonbonningtondata){reference-type="ref"
reference="fig:suttonbonningtondata"} we can see that the UK
Metrological Office statement that the file contains a "time series of
monthly data" is true, but the data is not useable "as is" due to the
way in which it is formatted. If we were to alter the observations it
contains into a series of columns labelled: Date, Maximum temperature,
Minimum temperature, etc., we would accept these as data for analysis,
and possibly even present this revised format as "raw data" in a formal
report with the justification that "nothing has been changed."

Our reason for emphasising these points are to lay the foundation for
introducing a terminology of "data state" to describe the changes in
data presentation that do not affect the inherent knowledge contained
within it. Looking ahead to
Table [1](#fig_terminology_introduction){reference-type="ref"
reference="fig_terminology_introduction"} and our terminology it is
clear that the data as supplied is what we term as Data *nascent* and
that the *Nominal* data fields are equivalent to the column format
proposed earlier in the previous paragraph. These interpretations are
based entirely on what we believe the data represents about the real
world and our analytical intent, if our research was to examine the
observational impact of changes in metrological recording equipment in
the accuracy of long term records, then we would clearly need to include
additional nominal data fields.

As the reader will see as the case study is developed, there has been a
noticeable trend in Sutton Bonnington local climate over the past 60
years which can be demonstrated by anyone who cares to examine the
public records. But a demonstration of an historical trend is does not
necessarily predict the future, which is why much scientific effort is
expended on developing predictive models.It is the goal of this work is
to demonstrate an effective approach to data driven analysis, so we
focus on the techniques used, rather than the analytical results.

The case study is available as a vignette at
<https://doi.org/10.5281/ZENODO.5994749>.

# The Definition Of Data  {#sec:the-definition-of-data-}



::: {#fig_terminology_introduction}
+----------------------------------------------------------------------+
| :   This term is used to describe the **presumed** data fields and   |
|     structure.                                                       |
|                                                                      |
|                                                                      |
|                                                                      |
| :   This is the **actual** initial state of data. Variances from the |
|     presumed nominal state are often described with pejorative terms |
|     such as "messy" and "untidy".                                    |
|                                                                      |
| :   Once transformed into a readable rectangular state, this raw     |
|     data is termed as data *s.l.* to emphasise that data may need    |
|     "cleaning" or other transformation before use. Multiple          |
|     instances of this state may be combined row wise to form a       |
|     larger data *s.l.* set.                                          |
|                                                                      |
| :   Once data are transformed into a fully defined state ready for   |
|     analysis it is termed data *s.s*. Multiple instances of this     |
|     state may be combined row wise to form a larger data *s.s.* set. |
|     However, if any data *s.l.* are included in such a combination,  |
|     the result are data *s.l.*                                       |
+:=====================================================================+
+----------------------------------------------------------------------+

: A lexicon of data states used in this work. Note that changing the
state does not require the creation or destruction of information.
:::

The definitions associated with our approach of data state are presented
in Table  [1](#fig_terminology_introduction){reference-type="ref"
reference="fig_terminology_introduction"}. The term "Nascent data" is
used in this work to distinguish it from the term "raw data", which is
frequently applied to data which has already been filtered and
transformed in some way, rather than to data which is truly in its "raw"
state. Justification for this viewpoint is found in work by
[@Bowker2013] whom collate a series of essays challenging presumptions
on the nature of raw data. These illustrate scenarios across multiple
scientific domains where "raw data" has been transformed and filtered by
processes applied prior to incorporation into a formal methodology. This
should not be seen as an attack on the *accuracy* of data used in the
scientific method, but instead is looking past the attractive simplicity
of idealised data into the underlying real world complexity, and the
need to describe *all* the processes needed to access that data.

With this introduction in mind, nascent data are conceptually
represented in data definition of
Figure [2](#Fig-theory-nascent-data){reference-type="ref"
reference="Fig-theory-nascent-data"}, after @Fox1994, as the starting
point for the application of the template approach developed in this
work.

The model for recording observations of the real world is to save data
as "Datum Triples", consisting of Entity, Attribute, and Value. An
Entity uniquely identifies the thing that is being observed. An
Attribute is the property being observed, and the Value may be
numerical, logical, categorical or any other representation of the
observation. This approach is versatile as it allows for the definition
of new attributes on-the-fly since it makes no presumption about the
final data format. The philosophical foundation for this approach in
information systems was explored by [@Dobson2001]. Using this reasoning,
our observations are used to build theories that are tested against the
real world leading to a better understanding of reality. We regard data
as observations and see the widely accepted "Dirty Data" viewpoint as
just one possible theory that describes *imperfect data*. We propose
here "Data State" as an alternative approach, and empirically test it
using the case study data.

As a component in a practical approach
Figure [2](#Fig-theory-nascent-data){reference-type="ref"
reference="Fig-theory-nascent-data"} implies a number of things:

-   Datum Triples are the fundamental unit for recording observations.

-   Data State relates to the organisation of Datum Triples.

-   Information is conserved when changing Data State. but

-   Knowledge may be more easily uncovered in particular data states.

-   Transformation of data for analysis differs from a change of state
    as it is not reversible.

The takeaway concept is the way in which real world observations are
transformed into a cloud of Datum Triples using the Data Model to form
our Nascent Data. Although we seldom see this actually used as the
primary method of data presentation as it is "not human readable", it is
functionally identical to the "long data" used as a transient format by
@Wickham2017 in their explanation of "Tidy Data Analysis". This provides
empirical evidence for generalising the data definition in
Figure [2](#Fig-theory-nascent-data){reference-type="ref"
reference="Fig-theory-nascent-data"} and its support within the R
programming language [@RCoreTeam2017].

We argue here that it is often a failure to correctly apply contextual
knowledge that results in the perception of "dirty data", and not always
an inherent fault in the data itself. Understanding how contextual
knowledge may be applied is the first step to generalising the
transformation of "Nascent data" into the desired state for analysis.
This concept is developed further in the following sections.

# Generalising Data Transformation {#sec:applying-the-data-model}

$$\label{eq:_disordered}
    \mathscr{D}_{Nascent} =
    \begin{pmatrix}
        d_{1,1} & d_{1,2} & \cdots & d_{1,n} \\
        d_{2,1} & d_{2,2} & \cdots & d_{2,n} \\
        \vdots  & \vdots  & \ddots & \vdots  \\
        d_{m,1} & d_{m,2} & \cdots & d_{m,n} 
    \end{pmatrix}$$

The generalised representation of nascent data is given in Equation
 [\[eq:\_disordered\]](#eq:_disordered){reference-type="ref"
reference="eq:_disordered"} where each element of the array represents
an item of data. It is tempting to consider that each column is a
consistent observation value, but the example of
Figure [1](#fig:suttonbonningtondata){reference-type="ref"
reference="fig:suttonbonningtondata"} illustrates how this need not be
case as it contains missing, annotated and incomplete representations of
values. An implicit assumption within this representation is that each
row represents a related set of observation values. Thus, there must
exist some form of unique identifier that associates groups of
observations although it is not actually shown as a value in Equation
 [\[eq:\_disordered\]](#eq:_disordered){reference-type="ref"
reference="eq:_disordered"}, although the row index $m$ acts as a proxy.

![A data definition that encompasses all states of data from real world
observations to final use. Domain specific assumptions using *a priori*
knowledge are used to guide the representation of real world nascent
data into a rectangular format.
](standalone-states-nascent.pdf){#Fig-theory-nascent-data
width="\\textwidth"}

$$\label{eq:_disordered_triples}
    \mathscr{D}_{Nascent} =
    \begin{pmatrix}
        d^{E} _{1} & d^{A} _{1}  & d^{V} _{1}   \\
        d^{E} _{2} & d^{A} _{2}  & d^{V} _{2}  \\
        \vdots  & \vdots  & \vdots   \\
        d^{E} _{p} & d^{A} _{p}  & d^{V} _{p}
    \end{pmatrix}$$

Where:\
$d^{E} _{p}  =$ datum Entity,\
$d^{A} _{p}  =$ datum Attribute, and\
$d^{V} _{p} =$ datum Value.\
The reason for emphasising the need for an identifier becomes clear as
we reorganise $\mathscr{D}_{Nascent}$ into columns of datum triples
*sensu lato* of the form in
Figure [2](#Fig-theory-nascent-data){reference-type="ref"
reference="Fig-theory-nascent-data"} and represented as
Equation [\[eq:\_disordered_triples\]](#eq:_disordered_triples){reference-type="ref"
reference="eq:_disordered_triples"}. Key to expressing data in this form
is the explicit definition of $d^{E}$ which allows the grouping of
related observations without the *a priori* definition of attributes and
values.

We can therefore generalise the representation of real world
observations using
Equation [\[eq:\_disordered_triples\]](#eq:_disordered_triples){reference-type="ref"
reference="eq:_disordered_triples"} as the template for the initial
data-state. This also enables the generalisation of all associated
analytical code since the only assumptions are the objects used to save
$d^{E}$, $d^{A}$ and $d^{V}$. However, we still require the need for
external knowledge to understand the factors that shaped the collation
of this data in order to interpret it.

This is illustrated with the case study data in
Table [2](#Tab:DatumTriples){reference-type="ref"
reference="Tab:DatumTriples"} where background knowledge and inductive
reasoning has been used to create a series of datum attributes to
describe the datum values. It is difficult to imagine a scenario which
would have been successful without *a priori* knowledge of the data
context. For example, the data are monthly figures so are *presumably*
valid for the last day of the month, so this value has been calculated
using external knowledge about calendars.

::: {#Tab:DatumTriples}
          datumEntity           datumAttribute        datumValue
  --------------------------- ------------------- -------------------
   Sutton.Bonnington:1961:02         YYYY         
   Sutton.Bonnington:1961:02          mm          
   Sutton.Bonnington:1961:02       tmax.degC      
   Sutton.Bonnington:1961:02       tmin.degC      
   Sutton.Bonnington:1961:02     airfrost.days    
   Sutton.Bonnington:1961:02        rain.mm       
   Sutton.Bonnington:1961:02       sun.hours      
   Sutton.Bonnington:1961:02       YYYYMMDD             -02-28
   Sutton.Bonnington:1961:02         place         Sutton Bonnington
   Sutton.Bonnington:1961:02       lattitude      
   Sutton.Bonnington:1961:02       logitude       
   Sutton.Bonnington:1961:02        easting       
   Sutton.Bonnington:1961:02       northing       
   Sutton.Bonnington:1961:02   height.amsl.metre  

  : Case Study: Presenting the data a datum triples requires an
  understanding and interpretation of the data within source text file.
:::

The long datum triple organisation of data may seem inconvenient for
researchers used to working with spreadsheets generating visualisation
directly from wide data, however, re-organisation between long and wide
formats a routine transformation for the flexible Tidyverse
[@Wickham2017]and GoG approach, where the verbs `pivot_longer` and
`pivot_wider` rearrange data between the two formats. Thus, Equations
[\[eq:\_disordered\]](#eq:_disordered){reference-type="ref"
reference="eq:_disordered"} and
[\[eq:\_disordered_triples\]](#eq:_disordered_triples){reference-type="ref"
reference="eq:_disordered_triples"} are equivalent projections of the
same data and we may use whichever form suits our analytic needs. To
clarify, adding data and transforming attributes are simplified in the
form of
Equation [\[eq:\_disordered_triples\]](#eq:_disordered_triples){reference-type="ref"
reference="eq:_disordered_triples"}. Tables and visualisations are best
generated with a wider format, which leads us to introduce other states
of data.

There are multiple valid representations of measurement units within
this simple data of the case study. Perhaps a UKNG (UK National Grid)
location format would be preferred; maybe the rainfall units were in
inches: or the sun,hours measured in minutes. The datum triple allows us
to add as many derived attributes as may be necessary by simply
appending more rows. If, for example, older rainfall data were in inches
and newer in millimetres, then additional attributes may be calculated
and appended in the desired units. We are not changing the knowledge
embedded in the data, but we are changing the *state* into something
more convenient.

To progress with the analysis we first reorganise
$\mathscr{D}_{Nascent}$ into columns of common data types *sensu lato*.
*Sensu lato* is used as a qualifier as the disordered data may use
multiple formats and units to encode the same data type. Transforming
$\mathscr{D}_{Nascent}$ into $\mathscr{A}_{Sensu \enspace lato}$ of
equation [\[eq:\_A_sensu_lato\]](#eq:_A_sensu_lato){reference-type="ref"
reference="eq:_A_sensu_lato"} is achieved by repositioning elements into
the desired arrangement.

Thus, we can see that the purpose of the data representation in
Figure [2](#Fig-theory-nascent-data){reference-type="ref"
reference="Fig-theory-nascent-data"} is one of identification and
grouping of the data elements, rather than transformation of element
contents. While mathematically trivial, we will show that this
re-ordering imparts properties that simplify the onward transformation
process and facilitates the construction of templates.

$$\label{eq:_A_sensu_lato}
    \mathscr{A}_{Sensu \enspace lato} =
    \begin{pmatrix}
        a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
        a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
        \vdots  & \vdots  & \ddots & \vdots  \\
        a_{m,1} & a_{m,2} & \cdots & a_{m,n} 
    \end{pmatrix}$$

Where the matrix elements $a_{m,n}$ represent data elements grouped as
rows of observations and columns of similar data types.

Note that the general form of Equation
[\[eq:\_A_sensu_lato\]](#eq:_A_sensu_lato){reference-type="ref"
reference="eq:_A_sensu_lato"} makes no assumption about the size of the
matrix, but when interpreted using the terminology of
Table [1](#fig_terminology_introduction){reference-type="ref"
reference="fig_terminology_introduction"} it should be clear that the
columns represent *nominal data fields* so multiple matrices of related
data *s.l.* will have identical columns. It makes no difference to the
information contained in the data if it is represented as a single large
or multiple small matrices. Thus, data *s.l.* can be divided or combined
row wise if they share nominal data fields, so even though the
definition is very loose, hence use of the term *sensu lato*, this
representation imparts properties to the data that were not present in
the source nascent data. These properties of data *s.l.* are now
examined in conjunction with reusable template actions.

![Essential characteristics of reusable templates in a reproducible
context. Raw data *sensu lato* is transformed into clean data *sensu
stricto* for reproducible
analysis.](template-map.pdf){#fig_essential_template
width="\\textwidth"}

Continuing with the same style of notation, data *sensu stricto* may be
defined as a matrix of the form in equation
[\[eq:\_B_sensu_stricto\]](#eq:_B_sensu_stricto){reference-type="ref"
reference="eq:_B_sensu_stricto"}. As with data *sensu lato* the rows
equate to observations, but now the nominal columns have all been
transformed to self-consistent formats equating to "tidy data".
Implicitly we are making an assumption that we can transform the
data *sensu lato* (equation
[\[eq:\_A_sensu_lato\]](#eq:_A_sensu_lato){reference-type="ref"
reference="eq:_A_sensu_lato"} ) into data *sensu stricto* (equation
[\[eq:\_B_sensu_stricto\]](#eq:_B_sensu_stricto){reference-type="ref"
reference="eq:_B_sensu_stricto"}), an assumption that will be justified
after the characteristics of data *sensu stricto* have been more fully
explored.

$$\label{eq:_B_sensu_stricto}
    \mathscr{B}_{Sensu \enspace stricto} =
    \begin{pmatrix}
        b_{1,1} & b_{1,2} & \cdots & b_{1,q} \\
        b_{2,1} & b_{2,2} & \cdots & b_{2,q} \\
        \vdots  & \vdots  & \ddots & \vdots  \\
        b_{p,1} & b_{p,2} & \cdots & b_{p,q} 
    \end{pmatrix} 
    \textrm{ where } m = p$$

The matrix elements $b_{p,q}$ represent data elements grouped as
transformed rows of observations and columns of common data types *sensu
stricto*. That is, for the same data, the total number of rows
data *sensu lato* $m$ are the same as the number of rows in the related
data *sensu stricto* $p$. While we can define the equivalence in the
number of rows as $m = p$, there is no basis on which to infer that
$q =  n$. Dates are encoded in one of the nominal fields, and while this
could be a single column with an ISO 8601 number string, other valid
single and multicolumn formats may be used for convenience: year, day
and week numbers might be encoded in separate columns. Other cases may
also arise along with derived fields included to facilitate analysis but
generally we would expect that $q \geq  n$.

In the preceding section, we state that there may be multiple matrices
of the form given by
equation ([\[eq:\_A_sensu_lato\]](#eq:_A_sensu_lato){reference-type="ref"
reference="eq:_A_sensu_lato"}) to cover the complete data. The same
reasoning may be applied to data *s. s.*, and since each
$\mathscr{B}_{Sensu \enspace stricto}$ has identical columns, they may
also be trivially combined row-wise.

$$\label{Eq:_test}
    s = 1 \geq
    \mathscr{T}_{Test}(a_{m,n}) \leq r \\
    \textrm{ selects from }
    \mathscr{T}_{Trans} =
    \left(
    \begin{array}{c}
        1 \\
        \vdots \\
        r
    \end{array} 
    \right) %...$$

> Where $\mathscr{T}_{Test}(a_{m,n})$ is a test for data type returning
> an index $s$, and $\mathscr{T}_{Trans}$ is a array of transformation
> functions.

We now introduce a function to describe the transformation of
$\mathscr{A}_{Sensu \enspace lato}$ into
$\mathscr{B}_{Sensu \enspace stricto}$ using mathematical
representations. These equations are not intended to be externally
justified, but instead should be interpreted as self consistent compact
descriptions of a novel process.
Equation ([\[Eq:\_test\]](#Eq:_test){reference-type="ref"
reference="Eq:_test"}) introduces a test that provides a single value
used as an index to select a transformation function from an array of
possible functions.

In the following equations a $\bullet$ operator is used to "pipe"
outputs from one function to the next in a chain. The purpose of using
this abstraction is to simplify the description of nested functions in
Equation [\[Eq:\_bullet_compact\]](#Eq:_bullet_compact){reference-type="ref"
reference="Eq:_bullet_compact"} from which
Equation [\[Eq:\_bullet\]](#Eq:_bullet){reference-type="ref"
reference="Eq:_bullet"} is derived. This operator is implemented in
programming languages, including R, so this representation is moving
towards a form that supports implementation. Once we have tested the
element and have determined an index for the transformation function,
the transformation process for each element may be represented in the
form of Equation [\[Eq:\_bullet\]](#Eq:_bullet){reference-type="ref"
reference="Eq:_bullet"}. $$\label{Eq:_bullet_compact}
    a_{m,n} \bullet \mathscr{T}_{Test}(a_{m,n}) \bullet \mathscr{T}_{Trans}(s) \equiv \mathscr{T}_{Trans}(a_{m,n} , \mathscr{T}_{Test}(a_{m,n}))$$
$$\label{Eq:_bullet}
    a_{m,n} \bullet \mathscr{T}_{Test} \bullet \mathscr{T}_{Trans} =
    b_{m,p}$$

But what happens if there is no transformation selected by
$\mathscr{T}_{Test}(a_{m,n})$ in Equation
[\[Eq:\_test\]](#Eq:_test){reference-type="ref" reference="Eq:_test"}?
Equation [\[Eq:\_bullet\]](#Eq:_bullet){reference-type="ref"
reference="Eq:_bullet"} is still valid, but the transformation does not
exist, so we can say that the *value* of $b_{m,p}$ is undefined. This
important definition will be used again after we iterate over the entire
matrix and to apply the transformation of
Equation [\[Eq:\_bullet_compact\]](#Eq:_bullet_compact){reference-type="ref"
reference="Eq:_bullet_compact"} to every element. As there is no
universal symbol for iteration, we define one here:
$$\label{Eq:_iterator}
    \forall x; x \in (1, \cdots, n) \equiv \prod_{1}^{n}$$
Eqation [\[Eq:\_iterator\]](#Eq:_iterator){reference-type="ref"
reference="Eq:_iterator"} may be literally be read as: "for all x ; x is
each value 1 to n." In this instance it can be simplified to: "x
iterates through each value 1 to n." This logic can be extended to nest
iterators as in
Equation [\[Eq:\_trans_lato_to_stricto\]](#Eq:_trans_lato_to_stricto){reference-type="ref"
reference="Eq:_trans_lato_to_stricto"}.

Thus, populating $\mathscr{B}_{Sensu \enspace stricto}$ may now be
represented by
Equation [\[Eq:\_trans_lato_to_stricto\]](#Eq:_trans_lato_to_stricto){reference-type="ref"
reference="Eq:_trans_lato_to_stricto"}.
$$\label{Eq:_trans_lato_to_stricto}
    \mathscr{B}_{Sensu \enspace stricto} = \prod_{1,1}^{m,n} a_{m,n} \bullet \mathscr{T}_{Test} \bullet \mathscr{T}_{Trans}$$

Note that
Equation [\[Eq:\_trans_lato_to_stricto\]](#Eq:_trans_lato_to_stricto){reference-type="ref"
reference="Eq:_trans_lato_to_stricto"} is inherently tolerant of
elements $a_{m,n}$ that cannot be transformed because
$\mathscr{T}_{test}$ does not return a value, since the matching
$b_{m,p}$ element values are undefined. Using the alternative
representation at matrix level is given by
Equation [\[Eq:\_matrix_lato_to_stricto\]](#Eq:_matrix_lato_to_stricto){reference-type="ref"
reference="Eq:_matrix_lato_to_stricto"}, the completeness of
transformation may be assessed by minimising the number of undefined
elements in $\mathscr{B}_{Sensu \enspace stricto}$.

$$\label{Eq:_matrix_lato_to_stricto}
    \mathscr{B}_{Sensu \enspace stricto} =  \mathscr{A}_{Sensu \enspace lato} \bullet \mathscr{T}_{Test} \bullet \mathscr{T}_{Trans}$$

We now have a mathematical description of linking the three states of
data, which may be expressed in the simplified form of
Equation [\[eq:\_three_states\]](#eq:_three_states){reference-type="ref"
reference="eq:_three_states"} where the arrows represent transformations
applied to matrix representations of data. Generally, the mathematical
approach used here will apply to any data where such transformations may
be defined.

$$\label{eq:_three_states}
    \mathscr{D}_{Nascent} \implies
    \mathscr{A}_{Sensu \enspace lato} \implies
    \mathscr{B}_{Sensu \enspace stricto}$$

# Introducing The Template Concept {#sec:template-concept}

The essential characteristics of a reusable template are represented in
Figure [3](#fig_essential_template){reference-type="ref"
reference="fig_essential_template"}. These have been creatively proposed
as theoretical models using an approach that is justified under the
Critical Realism umbrella as a novel theory [@Williams2018]. This is
achieved by constructing a hypothetical template and using thought
experiments to test its properties, then using the results to refine the
model. This iterative style of refinement and design is well established
in the systems engineering community as it presumes the existence of
poorly defined behaviours which must be incorporated into the final
system [@Ramos2010]. In the context of this work, it is the data which
is poorly defined, as revealed in the case study, and our template
system must cope with the uncertainty that this introduces. It is worth
reflecting for a moment how this contrasts with attempts to impose
inflexible structures on to data that inevitably result in "dirty data"
where datum cannot be easily mapped on to the imposed structure.

The template system in
Figure [3](#fig_essential_template){reference-type="ref"
reference="fig_essential_template"} explores multiple instances of same
reusable template in different contexts. It is important to understand
that this represents **multiple** applications of a **single** template
with many inputs of related data *s.l* outputting multiple sets of
data *sensu stricto*. The "Reproducible Analysis" accepts data *s. s.*
as its input for processing, however, it must be emphasised that the
output requires contextual understanding to be interpreted as knowledge.
For the purpose of generalising
Figure [3](#fig_essential_template){reference-type="ref"
reference="fig_essential_template"}, the "Reproducible Analysis" may be
thought of as reproducing the "number crunching" element of an
analytical task, prior to contextual interpretation. In the following
paragraphs these characteristics are explored in more detail to validate
the model by highlighting key parts of
Figure [3](#fig_essential_template){reference-type="ref"
reference="fig_essential_template"} in turn.

![The basic template functional process is highlighted in this diagram.
Multiple data *s.l.* are transformed by the template into data *s.s.*
which in turn are the focus for
analysis.](template-map-base-flow.pdf){#fig:template-map-base-flow
width="\\textwidth"}

Starting with the simple circumstance shown in
Figure [4](#fig:template-map-base-flow){reference-type="ref"
reference="fig:template-map-base-flow"}, two sets of data *s.l* are
transformed into a set of data *s.s*, which in turn are the focus for
analysis. A property that naturally flows from the matrix representation
of data from
Equation [\[eq:\_A_sensu_lato\]](#eq:_A_sensu_lato){reference-type="ref"
reference="eq:_A_sensu_lato"} and
Equation [\[eq:\_B_sensu_stricto\]](#eq:_B_sensu_stricto){reference-type="ref"
reference="eq:_B_sensu_stricto"} is the combination of multiple data
instances by "stacking" rows. We start to explore the useful
implications of this property in
Figure [5](#fig:template-map-combining-duplicate-data-2){reference-type="ref"
reference="fig:template-map-combining-duplicate-data-2"} where the same
template is used to twice and combines multiple sources of data *s.l.*.
Expressing this as a scenario inspired by current events [@Brodie2020]:
Imagine that the seven data *s.l.*. are daily COVID-19 data-sets that
have been combined as published as open data *s.s.*. If two more days of
data *s.l.* are privately available, they may be combined with the open
data *s.s.* and the reproducible analysis applied to investigate the
effects of this additional data.

![A property that naturally flows from the matrix representation of
data *s.l.* and data *s.s.* is the combination of multiple datum
rows.](template-map-combining-duplicate-data-2.pdf){#fig:template-map-combining-duplicate-data-2
width=".8\\textwidth"}

Staying with the same scenario as an explanatory tool,
Figure [6](#fig:template-map-combining-duplicate-data-1){reference-type="ref"
reference="fig:template-map-combining-duplicate-data-1"} illustrates how
the same data *s.s.* can be assembled by a researcher who only has
access to groups of data *s.l.*, but the final combined data *s.s.* is
identical in both
Figures [5](#fig:template-map-combining-duplicate-data-2){reference-type="ref"
reference="fig:template-map-combining-duplicate-data-2"} and [6](#fig:template-map-combining-duplicate-data-1){reference-type="ref"
reference="fig:template-map-combining-duplicate-data-1"}.

![The order in which data are combined with templates does not matter,
nor does the number of times that a template is
used.](template-map-combining-duplicate-data-1.pdf){#fig:template-map-combining-duplicate-data-1
width=".8\\textwidth"}

# Implementing A Template {#sec:implementation}

The compact representation of
Equation [\[Eq:\_matrix_lato_to_stricto\]](#Eq:_matrix_lato_to_stricto){reference-type="ref"
reference="Eq:_matrix_lato_to_stricto"} relates directly to the
reproducible template of
Figure [3](#fig_essential_template){reference-type="ref"
reference="fig_essential_template"} and provides justification for
asserting that such templates are feasible constructs. The portion of
Equation [\[Eq:\_matrix_lato_to_stricto\]](#Eq:_matrix_lato_to_stricto){reference-type="ref"
reference="Eq:_matrix_lato_to_stricto"} given by
Equation [\[Eq:\_template\]](#Eq:_template){reference-type="ref"
reference="Eq:_template"} eloquently captures the reusable template
functionality in a form that guides the practical implementation using a
programmatic language as conditional tests and transformations, with a
definition of what happens when no matching test is found.

$$\label{Eq:_template}
    \mathscr{T}_{Test} \bullet \mathscr{T}_{Trans}$$

The case study is a real world implementation of this theory that uses
`RMarkdowm` and `Bookdown` to implement a practical template. While
Figure [\[fig-modular-analysis-concept\]](#fig-modular-analysis-concept){reference-type="ref"
reference="fig-modular-analysis-concept"} shows the R code and reports
as neatly separated blocks, the two portions are intertwined together.

The property of row-wise combination of data *s.l.* allows us to treat
multiple instances of $\mathscr{A}_{Sensu \enspace lato}$ as a single
larger instance for the purpose of applying
Equation [\[Eq:\_matrix_lato_to_stricto\]](#Eq:_matrix_lato_to_stricto){reference-type="ref"
reference="Eq:_matrix_lato_to_stricto"}. Since it is also possible to
combine row-wise $\mathscr{B}_{Sensu \enspace stricto}$, all the implied
template characteristics in
Figure [3](#fig_essential_template){reference-type="ref"
reference="fig_essential_template"} may be met by appropriately
combining $\mathscr{A}_{Sensu \enspace lato}$ and
$\mathscr{B}_{Sensu \enspace stricto}$.

From
equation [\[eq:\_B_sensu_stricto\]](#eq:_B_sensu_stricto){reference-type="ref"
reference="eq:_B_sensu_stricto"}, failure to successfully transform an
element is assigned an undefined value which, although abstract is
easily tested within $\mathscr{B}_{Sensu \enspace stricto}$. Since we
are working with data too large to manually explore, it is clear that
some form of summary must be included within practical templates to
indicate successful transformation of elements. An appropriate
descriptive analysis from.

::: {.figure*}
![image](Modular_template_with_save.pdf){width=".8\\textwidth"}
:::

The theoretical framework for reusable templates constructed has so far
remained program language agnostic by using symbolic abstractions in the
form of equations. This has effectively deferred the formal selection of
a language until now. Choosing the R Studio ecosystem of software offers
an advantage in terms of stakeholder acceptance due to existing support
for academic style outputs using packages compatible with the approach
described in this work [@Allaire2019].

A general purpose reusable template concept is presented in
Figure [\[fig-modular-analysis-concept\]](#fig-modular-analysis-concept){reference-type="ref"
reference="fig-modular-analysis-concept"} which illustrates all the
functional components that are needed to support reusability. End-users
are not expected to interact with the transform elements at the code
level, although the intention is to write in a coding style that
supports end user modification. Rather, users will be given the
opportunity to use the template in conjunction with their own data in a
series of development micro-cycles, that implement feedback and
suggestions. The novel template theory developed here describes to
transformation of data through several states into data *s.s.* ready for
analysis, and once this data state has been achieved, the process is
well described. In
Figure [3](#fig_essential_template){reference-type="ref"
reference="fig_essential_template"} data *s.s* is tidy, well-formed and
may be regarded as the starting point for repeatable analysis.

# Template Evaluation

![Core reusable template functional flow. Each core step is component in
a practical reusable template based on the theory. Text marked in bold
indicates physical directories where working files are placed.
](core-reusable-framework.pdf){#fig_core_functional_flow
width=".5\\textwidth"}

The work by [@Cone2018; @Wickham2017] has done much to promote the use
of R, RStudio and R markdown as a literate programming environment
supporting development micro-cycles by running short code chunks
interactively and making amendments before committing to a full
compilation. These techniques are well described in texts on R, but it
should be emphasised that while there is much said about
reproducibility, there is currently little said about reusability.
However, we will demonstrate that the features of
Figure [\[fig-modular-analysis-concept\]](#fig-modular-analysis-concept){reference-type="ref"
reference="fig-modular-analysis-concept"} may be implemented in this
environment, and that stakeholders successfully incorporated such
implementations into their existing workflow as illustrated in
Figure [8](#fig:standalone-viewpoint){reference-type="ref"
reference="fig:standalone-viewpoint"}.

![The conventional approach sees data cleaning as a step required before
analysis may commence. The analytically fluent approach see the data
transformed from an initial inconvenient state into a convenient state
and analysis as a single series of process
steps.](standalone-viewpoint.pdf){#fig:standalone-viewpoint
width="0.5\\linewidth"}

While the details of every analysis will differ,
Figure [7](#fig_core_functional_flow){reference-type="ref"
reference="fig_core_functional_flow"} illustrates the core functional
flow that is common to all applications of the reusable template theory.
The purpose of this diagram is to provide a high level guide to
programmatic blocks of code that equate to the concepts presented in
Figure [\[fig-modular-analysis-concept\]](#fig-modular-analysis-concept){reference-type="ref"
reference="fig-modular-analysis-concept"}. Physical directory names are
provided to introduce a pragmatic standardisation of intermediate file
location. In a perfect world where code works first time such
conventions would have little utility, but in the real world where code
may interact in unexpected ways with real data, the ability to examine
each stage in the functional flow provides insights that can aid the
code debugging process.

Choosing to split the functional flow into a single or multiple sub
templates is application dependent dictated largely by time to run. The
author's choice was to work with code elements that took a maximum of
two minutes to run during development. This was highly dependent upon
source data format and size, but at times during the course of this
research up to 30 million rows of data were read for inclusion into
templates. Typically, the initial load stage was slowest, so it proved
convenient to produce data *s.l.* with one template and complete the
process to data *s.s.* with a second.

![RStudio supports the inclusion of user selectable templates in
packages as an installable extension to base R.
](templatemenu.png){#fig_RStudio_template_menu width="50%"}

The paths highlighted so far in
Figure [3](#fig_essential_template){reference-type="ref"
reference="fig_essential_template"} have carefully avoided duplicate
observations within data, but allowing the combining of multiple data
implies that the reusable template will see data *sensu lato* that
contains duplicates, thus requiring the definition of a functional
behaviour to address this issue.

![Combining data *sensu lato* and data *sensu stricto* for reproducible
analysis.](template-map-combining-data.pdf){#fig_essential_template_base_combining_data
width="6cm"}

The issue of duplicate data *s.l.* is explored in
Figure [\[fig_essential_template_duplicates\]](#fig_essential_template_duplicates){reference-type="ref"
reference="fig_essential_template_duplicates"} where we see that
combining data *sensu lato* should always result in identical data
irrespective of the permutation data are transformed and combined
through the reproducible template. If duplicate data are simply included
then the length of data will depend on the path through data and the
number of duplicates. With Big Data this may act as an unacceptable
multiplier on data size.

A better approach is to introduce a metadata field that is used to
describe characteristics such as source, transforms and duplicates
relating to the data as a single entry. Thus, the multiple paths shown
in
Figure [\[fig_essential_template_duplicates\]](#fig_essential_template_duplicates){reference-type="ref"
reference="fig_essential_template_duplicates"} result in identical data
*sensu stricto*, but slightly different metadata, capturing the
transformations applied. While this refinement is not strictly necessary
for any single analysis, it keeps a permanent link to the source at the
record level available for an as yet unspecified analysis.

# Discussion {#sec:discussion}

This work challenges the conventional view of "dirty data" impeding the
analytical process and demonstrates how an alternative viewpoint can
pave the way for new approaches. There is no doubt that data can be
flawed in many ways and great progress has been made in the theoretical
understanding with addressing such issues, for example:
@Konstantinou2019
[@Loo2018; @Almeida2018; @Hill2017; @Singh2017; @Rammelaere2017; @Fanani2017].
Practical tools have also been demonstrated: @OpenRefine2018
[@WinPure; @Quadient2018; @Gubanov2017; @Mecca2017; @Xu2017; @Chu2015].
However, we argue here that it is a mistake to confound data that is
inconveniently presented with problems in data quality. The forward
thinking of authors such as @Hoare1975 [@Brodie1980; @Fox1994], made
before the age of "Big Data" are helpful for their clarity of
presentation, and provided groundwork for the viewpoint used here.
Inspiration for developing reusable templates was found in the "Literate
Programming" of @Knuth1984 and @Gentleman2005. Gentleman has played a
key role in the development of R language [@RCoreTeam2017] which has
support for templates and Literate Programming techniques.

RStudio supports the inclusion of user selectable templates in packages
as an installable extension to base R, as illustrated in
Figure [9](#fig_RStudio_template_menu){reference-type="ref"
reference="fig_RStudio_template_menu"}, ensuring that dependencies and
custom functions are also installed. Enabling this capability requires
that all code passes the validation checks provided by [@RCoreTeam2017].
This point is made here in part to retrospectively justify the choice
made for implementation, and in part to explain why no user interface
was developed: it was already there. What was missing was a coherent
theory of how to implement reusability, which was provided by this
research. As a final technical point, templates may embed the code that
call the required libraries directly and ensure that they are correctly
installed, as such, they may be circulated as stand-alone entities and
independently extended by programmatically skilled users.

# Conclusions

This work has demonstrated that processes grouped under the umbrella
term "Data Cleaning", may be generalised by re-framing as "Data State".
This abstract device facilitates the development of reusable code
elements as templates. The foundations for this novel approach are
justified through the work of authors such as @Fox1994 for their
definition of data, and the work of [@Bhaskar2008] for interpretation
and theory building. The practical application of this work is in the
analysis of data too large to manually handle, that is, "Big Data".

[^1]: The authors thank EPSRC grant: EP/R513088/1 for funding the
    doctoral research on which this work is based.

[^2]: By "Big Data" we mean data that is too big to be conveniently
    manipulated by manually intensive methods.

[^3]: The philosophical basis for this interpretation to is described in
    Section [4](#sec:the-definition-of-data-){reference-type="ref"
    reference="sec:the-definition-of-data-"}

[^4]: The location was chosen because it was the closest to Loughborough
    University, Leicestershire, UK.
