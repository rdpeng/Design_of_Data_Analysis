# Systems Thinking



## Learning Objectives {-}


## Introduction

The presentation of how data analyses are conducted is typically done in
a forward manner. A question is posed, data are collected, and given the
question and data, a system of statistical methods is assembled to
produce evidence. That evidence is then interpreted in the context of the
original question. While such a description provides a useful model, it
is incomplete in that it assumes the statistical methods are completely
determined by the question and the data. In practice, there is an
equally important "backwards\" process that data analysts use to either
revise their statistical approach or investigate potential problems with
the data. This process of revision is driven by observing deviations
between the data and what we expect the data to look like.

Much previous work dedicated to studying the data analysis process has
focused on the notion of "statistical thinking,\" or the cognitive
processes that occur within the analyst while doing data
analysis [@wildpfannkuch1999; @grol:wick:2014; @wats:call:2003; @hortonhardin2015].
Grolemund and Wickham [@grol:wick:2014] refer to these "forwards\" and
"backwards\" aspects of data analysis as part of a sensemaking process,
where analysts attempt to reconcile schema that describe the world with
data that are measured from the world. Should there be a discrepancy
between the schema and the data, the analyst can update the schema to
better match the observed data. These updates ultimately result in
knowledge being accumulated.

A key task for data analysts is to interpret the scientific question at
hand, the available data, context, and resources, and assemble a system
of statistical methods to analyze the data to produce a evidence. In
general, statistical theory provides guidance on which methods should be
applied in different data analytic scenarios. With knowledge of the
context, scientific question, and the system of methods assembled, the
data analyst can generate certain expectations for what the results of
the analysis will be. However, once those methods are applied to the
data and results are obtained, there is comparatively little formal
methodology for reconciling differences between the observed results and
our analytic expectations. In particular, knowledge of the cognitive
process of statistical thinking does not suggest how to give feedback to
analysts once the data are observed. While there is much previous work
on data analysis assessments [@Chance1997; @Garfield2000; @Chance2002]
and project-based
learning [@Gnanadesikan1997; @Smith1998; @Tishkovskaya2012], the
literature provides little insight into how to recognize or formally
assess why a data analysis has produced an unexpected outcome.

The question of how analysts should reconcile discrepancies between
their expectations about the world and the observed data is the focus of
this paper. We propose that a framework for characterizing this process
emerges by considering the sequence of statistical methods being applied
to the data as a *system* that induces a set of expected outcomes. This
system then generates outputs that may deviate from our expectations and
these deviations are referred to as *anomalies*. In a typical data
analysis there may be many anomalies and the analyst must identify their
root causes in order to determine the next step in the analysis.

Characterizing anomalies in data analysis requires that we have a
thorough understanding of the entire system of statistical methods that
are being applied to a given dataset. Included in this system are
traditional statistical tools such as linear regression or hypothesis
tests, as well as methods for reading data from their raw source,
pre-processing, feature extraction, post-processing, and any final
visualization tools that are applied to the model output. Ultimately,
anomalies can be caused by any component of the system, not just the
statistical model, and it is the data analyst's job to understand the
behavior of the entire system. Yet, there is little in the statistical
literature that considers the complexity of such systems and how they
might behave under real-world conditions [@national1991future].

Shifting to a framework that focuses on anomalies in data analysis opens
up new avenues for thinking about the data analysis process more
generally. In particular, we can apply approaches from systems
engineering to formally analyze how data analysts think about a problem.
Tools like fault trees can provide a way of evaluating how well analysts
understand the complex systems being applied to their data and can
suggest improvements to those systems [@vesely1981fault]. Comparing
systems of statistical methods based on how they produce anomalies
provides a new basis for choosing between approaches, in addition to
traditional metrics such as efficiency or prediction accuracy. Finally,
diagnosing data analytic anomalies provides a direct and generalizable
approach to teaching data analysis on a large scale.

In this paper we introduce the concept of a statistical methods system,
which is a collection of statistical tools that are applied to a dataset
for answering a given question. We then discuss the notion of a set of
expected outcomes and an anomaly space that is determined by the outputs
of the system and the expected outcomes. Finally, we describe fault
trees and how they can be used to analyze a statistical methods system
with respect to different anomalies. We also provide some examples of
how fault trees can be used to compare different statistical methods
systems and choose between them.

## Statistical Methods Systems

To study the manner in which data analyses can produced unexpected
results, it is useful to first consider data analyses as a system of
connected components that produce specific outputs. A *statistical
methods system* is a collection of data analytic elements, procedures,
and tools that are connected together to produce a data analysis
*output*, such as a plot, summary statistic, parameter estimate, or
other statistical quantity. By connecting these elements and tools
together, we create a complex system through which data are transformed,
summarized, visualized, and
modeled [@hick:peng:2019; @Breiman2001cultures]. Each of the components
in the system will have its own inputs and outputs and tracing the path
of those intermediate results plays a key role in developing an
understanding of the system.

There are also contextual inputs to the data analysis, such as the main
question or problem being addressed, the data, the choice of programming
language to use, the audience, and the document or container for the
analysis, such as Jupyter or R Notebooks. While these inputs are not
necessarily decided or fundamentally modified by the analyst, the data
analyst may be expected to provide feedback on some of these inputs. In
particular, should an analysis produce an unexpected result, the analyst
might identify one of these contextual inputs as the root cause of the
problem.

A statistical methods system can be characterized as a sequence of steps
or a deterministic algorithm. The algorithm is not necessarily
uni-directional and may double-back on itself and branch into multiple
sections. Ultimately, the algorithm produces an output that may be
interpreted by the analyst or passed on to serve as input to another
system. In describing the behavior of any system, one must be careful to
define the resolution of the system (i.e. how detailed we want to
specify steps) and the boundaries of the system diagram. In particular,
we should acknowledge what elements are excluded from the diagram, such
as application-specific context or other assumptions [@vesely1981fault].
The development of a statistical methods system would typically be
guided by statistical theory, as well as knowledge of the scientific
question, any relevant context or previous research, available
resources, and any design requirements for the system output.

### Example: A Simple Linear Model System

Consider a system that fits a simple linear model as part of a data
analysis. This system reads in some data on a pair of variables $x$ and
$y$, fits a simple linear regression via least squares, and outputs the
intercept and slope estimates. A depiction of this system along with
some representative R code is shown in
Figure [1](#modelSLR){reference-type="ref" reference="modelSLR"}.

![Simple linear regression statistical methods system with
pseudo-code.](modelSLR.png){#modelSLR width="4in"}

The diagram indicates that understanding how this system operates
requires knowledge of (1) how the data are read in; (2) how the model is
fit to the data; and (3) how the estimated coefficients are extracted
from the model fit and outputted. Specifically, if we are using R to
analyze the data, we must have an understanding of the `read.csv()`,
`lm()`, `coef()`, and `print()` functions.

## Set of Expected Outcomes {#sec:expectations}

Once a statistical methods system is built, but before it is applied to
the data, we can develop a set of *expected outcomes* for the system.
These expected outcomes represent our current state of scientific
knowledge and may reflect any gaps or biases in our understanding of the
data, the problem at hand, or the behavior of the statistical methods
system. The overarching goal of the data analysis is to produce outputs
that will in some way improve our understanding of the scientific
problem. Without any expected outcomes, it is challenging to interpret
the output of the system or determine how the output informs our
understanding of the underlying data generation process.

An important property of the set of expected outcomes is that the
expected outcomes are always stated in terms of the observed output of
the system, *not* any underlying unobserved population parameters. We
draw a distinction here between *hypotheses*, which are statements about
the underlying population, and *expected outcomes*, which are statements
about the observed sample data. Another property of the set of expected
outcomes is that they will generally have sharp boundaries. Therefore,
once we observe the output from the statistical methods system, we know
immediately and with complete certainty whether the output is expected
or unexpected. Boundaries of this nature are important for the analyst
so that decisions can be made regarding any next steps in the analysis.

Developing a useful set of expected outcomes is part of the design
process for a statistical methods system and depends on many factors,
including our knowledge and assumptions about the underlying data
generation process, our ability to model that process using statistical
tools, our knowledge of the theoretical properties of those tools, and
our understanding of the uncertainty or variability of the observed data
across multiple samples. Hence, even though the underlying truth might
be thought of as fixed, it is reasonable to assume that different
analysts might develop different sets of expected outcomes, reflecting
differing levels of familiarity with the various factors involved and
different biases towards existing evidence or data.

### Example: Expected Outcomes for the Sample Mean {#sec:expectedmean}

In some contexts, the set of expected outcomes may be derived from
formal statistical hypotheses. For example, we may design a system to
compute the sample mean $\bar{x}$ of a dataset $x_1,\dots,x_n$ that we
model as being sampled independently from a $\mathcal{N}(\mu,1)$
distribution. In this case, the output from the system is $\bar{x}$ and
based on our current knowledge we may hypothesize that $\mu=0$. Under
that hypothesis, we might expect $\bar{x}$ to fall between $-2/\sqrt{n}$
and $2/\sqrt{n}$. For $n=10$, this interval is $[-0.63,0.63]$ and any
observed value of $\bar{x}$ outside that interval would be an anomaly.

Another analyst might be more familiar with this data generation process
and therefore hypothesize that the underlying population mean is $\mu=3$
without assuming a Normal distribution. This analyst might also know
that the data collection process can be problematic, leading to very
large observations on occasion. Therefore, based on experience and
intuition, this analyst has a wider expected outcome interval of
$[1, 5]$.

In both examples here, the set of expected outcomes was a statement
about $\bar{x}$, the output of the system applied to the observed data.
The set of expected outcomes was also a fixed interval with clear
boundaries, making it straightforward to determine whether the output
would fall in the interval or not.

## Anomaly Space {#sec:faultpoints}

An anomaly occurs only if there is a clearly defined set of expected
outcomes for a system, and the observed output from the system does not
fall into that set. The specification of an anomaly then requires three
separate elements:

1.  A description of *what* specific system output or collection of
    outputs is observed;

2.  A description of *how* the outputs deviate from their expected
    outcomes; and

3.  An indication of *when* or under what conditions the deviation is
    observed.

Continuing the example from
Section [2.2.1](#sec:expectedmean){reference-type="ref"
reference="sec:expectedmean"} above, an anomaly for the sample mean
could be "$\bar{x}$ is outside the expected interval of $[-0.63, 0.63]$
when a sample of size $n=10$ is inputted to the system". The observed
output is $\bar{x}$, the deviation is "outside the interval
$[-0.63, 0.63]$\", and the event occurs when $n=10$.

The *anomaly space* of a statistical methods system consists of the
collection of potential outputs from the system which would indicate
that an anomaly has occurred. Fundamentally, the anomaly space is the
complement of the set of expected outcomes. Not all areas of the anomaly
space are equally important and in some applications it may be that
anomalies occurring in certain subsets of the anomaly space are more
interesting than anomalies occurring elsewhere. The size of the anomaly
space of a statistical methods system is determined by the outputs
produced by the system. Looking back to the simple linear model system
in Figure [1](#modelSLR){reference-type="ref" reference="modelSLR"},
there are only two outputs ($\hat{\beta}_0$ and $\hat{\beta}_1$) that
define the anomaly space. Therefore, any anomalies for that system must
be determined by those two values.

As the number of system outputs grows, the size of the anomaly space may
grow accordingly. For example, we can increase the size of the anomaly
space for the simple linear model system by also returning the standard
errors of the coefficients. With each additional output, we increase the
number of ways in which anomalies can occur. Different systems with
different sets of outputs will induce anomaly spaces of differing sizes
and the nature of the anomaly space associated with a system may serve
as a factor in choosing between systems. Also, because the anomaly space
depends on the specification of the set of expected outcomes, different
analysts with different expectations could induce different anomaly
spaces for the same statistical methods system.

Once a statistical methods system has been applied to the data and an
anomaly has been observed, the "forward\" aspect of data analysis is
complete and the analyst must begin the "backward\" aspect to determine
what if any changes should be made to the analysis. Such changes could
involve modifying the statistical methods system itself or could require
changes to our set of expected outcomes based on this new information.
However, before any decision can be made in response to observing an
anomaly, a data analyst must enumerate the possible *root causes* of the
anomaly and determine which root causes should be investigated.

## Fault Trees {#sec:faulttrees}

The key formal tool that we introduce here is the *fault tree*, which
can be used to analyze the design and operation of a statistical methods
system and to identify root causes for any system anomalies. A fault
tree is a tool commonly used in systems engineering for conducting a
structured risk assessment and has a long history in aviation,
aerospace, and nuclear power
applications [@vesely1981fault; @michael2002fault]. In general, fault
trees can be used to discover the root cause of an anomaly after it
occurs. However, an important use case for fault trees is to develop a
comprehensive understanding of a system before an anomaly occurs. As
such, it is a valuable design tool for complex systems.

A fault tree is a graphical tool that describes the possible
combinations of causes and effects that lead to an anomaly. At the top
of the tree is a description of an anomaly. The subsequent branches of
the tree below the top event indicate possible causes of the event
immediately above it in the tree. The tree can then be built recursively
until we reach a root cause that cannot be further investigated. Each
level of the tree is connected together using logic gates such as AND
and OR gates. The leaf nodes of the tree indicate the root causes that
may lead to an anomaly. Comprehensive details on constructing fault
trees can be found in Vesely, et al. [@vesely1981fault] and
Ericson [@ericson2011fault].

In principle, a fault tree can be constructed for every potential system
anomaly that could be observed. However, if the goal is to develop an
understanding of the strengths and weaknesses of a statistical methods
system's design, then it may be desirable to identify a few important
anomalies whose fault trees provide substantial visibility into the
operating characteristics of the system [@vesely1981fault].

Example: Fault Tree for a Simple Linear Model System
----------------------------------------------------

We can build a fault tree for the simple linear model system shown in
Figure [1](#modelSLR){reference-type="ref" reference="modelSLR"}. Under
normal operation, we might expect that $\hat{\beta}_1\approx 1$. With
typical random variation in the data we might expect $\hat{\beta}_1$ to
range from 0 to about 3. Therefore, it would be unexpected to observe
$\hat{\beta}_1<0$ or $\hat{\beta}_1>3$. For this example, we will define
the anomaly of interest as "$\hat{\beta}_1 < 0$ when outputted from the
model fit\". Note that although the set of expected outcomes is the
interval $[0, 3]$, we define the anomaly as $\hat{\beta}_1<0$ and ignore
the part of the anomaly space defined by $\hat{\beta}_1>3$. Similarly,
possible anomalies concerning the intercept $\hat{\beta}_0$ will not be
developed here.

Starting with the system description in
Figure [1](#modelSLR){reference-type="ref" reference="modelSLR"}, we can
see that should we observe $\hat{\beta}_1<0$ there are two possibilities
we might consider: (1) the structural relationship between $x$ and $y$
has changed to no longer reflect the simple linear model; or (2) the
underlying structural relationship remains, but the input data to the
linear model has been contaminated, perhaps with outliers. Developing
the "contaminated data\" event a bit further, we can propose that either
there are outliers present in the raw data or outliers were somehow
introduced into the data before inputting to the regression model.

The completed fault tree is shown in
Figure [2](#ftreeSLR){reference-type="ref" reference="ftreeSLR"} and was
built using the `FaultTree` package in R [@faulttreeRpackage2020]. The
leaf nodes are labeled with circles to indicate the root cause events.

![Fault tree for unexpected event of "Estimated coefficients outside of
expected range when model fitted to the input
data.\"](ftreeSLR.png){#ftreeSLR width="6in"}

For a description of the notation and symbols used in the fault tree,
see Appendix [7](#sec:notation){reference-type="ref"
reference="sec:notation"}. The fault tree indicates that outliers can be
a root cause of the anomaly (events $E_{11}$ or $E_{12}$). However,
outliers do not *always* cause an unexpected change in $\hat{\beta}_1$.
The AND gate at $G_5$ indicates that the outliers also have to be
arranged in such a manner that they cause $\hat{\beta}_1$ to be $< 0$,
perhaps because of some selection process in the outlier
generation ($E_{13}$).

The other root cause is that there is a change in the underlying
structural relationship between $x$ and $y$ or that we perhaps
misunderstood the relationship between $x$ and $y$ in the first place.
For example, it could be that the data are naturally more variable than
we thought they were, and so our set of expected outcomes for
$\hat{\beta}_1$ should be larger. In any case, the AND gate at $G_6$
indicates that any structural change also needs to be large enough
(relative to the noise in the data) so that we are able to observe it in
the data.

### Minimal Cutsets

A useful summary statistic from any properly constructed fault tree is
the list of minimal cutsets, which is a summary of the minimal number of
events that need to happen in order for the anomaly to occur. There may
be many cutsets and there may be different cutsets with 1, 2, or more
events in them. The fault tree in
Figure [2](#ftreeSLR){reference-type="ref" reference="ftreeSLR"} has
three cutsets of size 2: $\{E_{11}, E_{13}\}$, $\{E_{12}, E_{13}\}$, and
$\{E_7, E_8\}$. An example of one cutset is (1) There are outliers in
the raw data (event $E_{11}$), AND (2) a selection process arranges the
outliers such that they produce $\hat{\beta}_1<0$ (event $E_{13}$). If
this pair of events occurs, the system will experience the anomaly
described at the top of the tree. Given the specification of a fault
tree, minimal cutsets can generally be computed automatically using
standard rules of boolean algebra [@michael2002fault].

## Exploratory vs. Confirmatory Methods Systems

The anomaly space of the simple linear model system in
Figure [1](#modelSLR){reference-type="ref" reference="modelSLR"} is
completely determined by the values of $\hat{\beta}_0$ or
$\hat{\beta}_1$. The specification of the anomaly space here therefore
rules out other possible anomalies that may be of interest when applying
a linear model. For example, an anomaly such as "One or more regression
model residuals are larger than expected when outputted from the model
fit\" is not contained within the anomaly space of the system, because
the regression model residuals are not part of the system output. If
there are unusually large residuals from the model fit, they can only
cause an anomaly in this system inasmuch as they affect the values of
$\hat{\beta}_0$ and $\hat{\beta}_1$. Whether this feature is a strength
or a weakness of the system depends on the application context, but it
suggests that the structure of the anomaly space should be considered
when designing a statistical methods system.

### A Scatterplot+Regression Line System

Suppose that instead of the simple linear model system we used a
scatterplot as our statistical method system, with the plot of the data
and the overlaid simple linear regression line being the output of
interest. This system is shown in
Figure [3](#modelPlot){reference-type="ref" reference="modelPlot"}.

![Scatterplot with overlaid regression line statistical methods system
with pseudo-code.](modelPlot.png){#modelPlot width="5in"}

The scatterplot system does not output the estimated regression
coefficients (they are intermediate outputs) but rather passes them into
a plotting routine that can overlay the fitted regression line.

The scatterplot system in Figure [3](#modelPlot){reference-type="ref"
reference="modelPlot"} includes a plot of the data along with the
regression line, which is considerably more revealing than the
regression coefficients alone. The anomaly space of this system contains
at least the following events:

1.  Nonlinear structure visible in residuals;

2.  Model residuals out of expected range (outliers in the $y$
    direction);

3.  Model leverages out of expected range (outliers in the $x$
    direction);

4.  Intercept or slope of regression line out of expected
    range[\[failureSLR\]]{#failureSLR label="failureSLR"}.

Note that the anomaly space of the simple linear model system is
included within the anomaly space of the scatterplot+regression line
system (i.e. case [\[failureSLR\]](#failureSLR){reference-type="ref"
reference="failureSLR"}). Because of the graphical nature of the output,
all of the anomaly assessments in the scatterplot+regression line system
must be made by eye.

One anomaly we can consider is "A model residual is out of the expected
range when viewed on the scatterplot\".
Figure [4](#ftreePlot){reference-type="ref" reference="ftreePlot"} shows
the fault tree for the scatterplot+regression line system and this
anomaly. Here, we only consider the case where the residual is positive.

![Fault tree for unexpected event of "Model residuals out of expected
range.\"](ftreePlot.png){#ftreePlot width="6.25in"}

Given that a large positive residual implies that $y_i >> \hat{y}_i$,
the source of the problem lies with either the data $y_i$ or the model
output $\hat{y}_i$. Possible problems with the data $y_i$ include errors
in the raw data (such as a corrupted file), introduction of error upon
reading the data, or some type of measurement problem in the process
generating the raw data. Regarding problems with the modeling, it is
possible that the model does not capture an important nonlinearity, is
missing a key covariate (and the observation is an outlier in that
covariate), or that the error structure is incorrectly specified
(possibly non-normal or heteroscedastic).

## Comparison of Systems

A key difference between the simple linear model system in
Figure [1](#modelSLR){reference-type="ref" reference="modelSLR"} and the
scatterplot+regression line system in
Figure [3](#modelPlot){reference-type="ref" reference="modelPlot"} is
the potential size of their respective anomaly spaces. The anomaly space
of the simple linear model system is determined by the estimated
coefficients from the model while the anomaly space of the scatterplot
system is determined by the fitted linear regression line and all of the
data points.

The difference between how these two systems can produce anomalies is
notably demonstrated in Anscombe's famous Quartet, which consists of
four sets of data on hypothetical variables $x$ and $y$. In each of the
four datasets, key summary statistics such as the number of
observations, Pearson correlation between $x$ and $y$, slope of the
simple linear regression line, and regression sums of squares are
identical [@anscombe1973graphs]. If one were to rely on these numerical
summaries alone to characterize the data, one would conclude that all of
the datasets were the same.

We reproduce Anscombe's original scatterplots below in
Figure [5](#anscombeplot){reference-type="ref"
reference="anscombeplot"}. Upon seeing the four datasets, it is clear
that the datasets are not identical and present some features that were
perhaps unexpected.

![Simple linear regression statistical methods
system.](anscombeplot.pdf){#anscombeplot width="5in"}

Each of the scatterplots in
Figure [5](#anscombeplot){reference-type="ref"
reference="anscombeplot"}B--D presents an anomaly if one's set of
expected outcomes specifies that the observed data $y$ follow the form
$y=\beta_0+\beta_1x+\varepsilon$ under the usual linear model
assumptions:

1.  Figure Figure [5](#anscombeplot){reference-type="ref"
    reference="anscombeplot"}A does not present an anomaly; it is what
    one might expect to observe from a simple linear model;

2.  Figure Figure [5](#anscombeplot){reference-type="ref"
    reference="anscombeplot"}B is anomalous because the data exhibit a
    clear nonlinear (possibly quadratic) pattern in addition to a linear
    trend;

3.  Figure Figure [5](#anscombeplot){reference-type="ref"
    reference="anscombeplot"}C is anomalous because the data have an
    outlier in the $y$-direction;

4.  Figure Figure [5](#anscombeplot){reference-type="ref"
    reference="anscombeplot"}D is anomalous because the data have an
    outlier in the $x$-direction.

In each of Figures [5](#anscombeplot){reference-type="ref"
reference="anscombeplot"}B--[5](#anscombeplot){reference-type="ref"
reference="anscombeplot"}D above, the system output
(scatterplot+regression line) provides a useful indicator that the
simple linear model is inadequate for describing the dataset being
plotted.

Anscombe used this quartet of examples as part of an argument for
graphing the data versus only computing numerical summaries. The
rationale for his argument is not clearly articulated and is along the
lines of "We know it when we see it". But it is possible to formally
compare the two systems he proposes (numerical summaries vs.
scatterplots) based on an analysis of their anomaly spaces and their
fault trees.

The value of the scatterplot+regression line approach in this situation
is that its large anomaly space offers many opportunities to interrogate
the data analytic process by observing anomalies and question our
understanding of the data generation process. When we compare the fault
trees in Figure [2](#ftreeSLR){reference-type="ref"
reference="ftreeSLR"} and Figure [4](#ftreePlot){reference-type="ref"
reference="ftreePlot"} we can see that for the simple linear model,
outliers (i.e. large model residuals) represent a root cause of an
anomaly related to $\hat{\beta}_1$. However, for the
scatterplot+regression line system, large model residuals are a
top-level anomaly in and of themselves. Because this top-level anomaly
for the scatterplot+regression line system is an "upstream\" root cause
for the simple linear model, the scatterplot+regression line can trigger
an anomaly earlier in the data analytic process than the simple linear
model system can.

The difference in the structure of the fault trees in
Figures [2](#ftreeSLR){reference-type="ref" reference="ftreeSLR"}
and [4](#ftreePlot){reference-type="ref" reference="ftreePlot"} does not
indicate that one approach is good and the other is bad, but it does
provide a formal characterization of their strengths and weaknesses.
Determining which fault tree (and therefore, which system) is more
useful depends in part on our confidence in our understanding of the
underlying process that generates the data.

### Characteristics of Exploratory and Confirmatory Systems

The simple linear model system that focuses solely on the regression
coefficients is only appropriate in situations where one has decided
that the coefficients are the sole quantity of interest and that unusual
patterns in the data are tolerable to some degree. This system might be
part of a "confirmatory analysis\" where problems with the data, such as
outliers, are free to occur and are only relevant if they cause an
unexpected problem with the estimated coefficients. However, if one is
in the phase of data analysis where the data generation process is still
being investigated (i.e. "exploratory analysis\") then it would make
more sense to choose the scatterplot+regression line system. This system
has a large anomaly space and a variety of ways in which anomalies can
occur. Therefore, unexpected problems with the data or the modeling can
easily be surfaced and possibly rectified by the data analyst.

Characterizing statistical methods systems via their anomaly spaces and
fault trees allows us to develop the following generalization with
respect to exploratory and confirmatory methods systems:

1.  An *exploratory statistical methods system* has a large anomaly
    space and the fault trees associated with the anomalies have minimal
    cutsets that are small in size or prioritize single event root
    causes;

2.  A *confirmatory statistical methods system* has a small anomaly
    space and the fault trees associated with each of the anomalies have
    minimal cutsets that are large in size and few (if any) single event
    root causes.

Exploratory systems tend to prioritize visualization or related
procedures where the number of outputs is on the order of the sample
size itself [@tukey1977exploratory; @cham:clev:1983]. Such approaches
provide ample opportunities to detect deviations from expected outcomes
in situations where we are learning about the data and the processes
underlying them.

Confirmatory systems tend to prioritize robustness and nonparametric
flexibility because their outputs are small in number and must be
reliable [@hube:1977; @tsiatis2007semiparametric]. Once we have
determined which outputs are of interest, we typically want to refine
the system to focus on those outputs; increasing the size of the anomaly
space may serve as more of a distraction in this type of analysis.
Critical to confirmatory statistical methods systems is their ability to
protect against failures that are fundamentally unobservable within the
context of the data analysis.

A typical pattern that might be followed in going from an exploratory to
confirmatory system would be to start with an exploratory system applied
to a preliminary dataset. Using a highly iterative process, the
exploratory system could reveal a number of anomalies and allow the
analyst to reconsider the set of expected outcomes and the design of the
statistical methods system. Once the analyst has developed a deeper
understanding of the data generation process, a separate (perhaps
larger) effort could be made to collect new data. At this point, a
confirmatory statistical methods system could be applied to analyze
those new data, incorporating the lessons learned from the exploratory
analysis.

### From Exploratory to Confirmatory: "Fixing" Anscombe's Quartet 

Upon seeing the three types of anomalies indicated by the scatterplots
in Anscombe's Quartet (Figure [5](#anscombeplot){reference-type="ref"
reference="anscombeplot"}B--[5](#anscombeplot){reference-type="ref"
reference="anscombeplot"}D), we could attempt to revise the simple
linear model system in Figure [1](#modelSLR){reference-type="ref"
reference="modelSLR"} so that the output would be more reliable and
trustworthy when applied to future datasets of this nature. The
exploratory analysis based on the scatterplot provided useful
information regarding the design of our statistical methods system that
we can now incorporate into a revision.

One way we could modify our original system is to design a system that
only produces outputs when the data appear as in
Figure [5](#anscombeplot){reference-type="ref"
reference="anscombeplot"}A. That way, we could be reasonably confident
that there isn't significant nonlinear structure or large outliers.
Based on our "experience\" with
Figure [5](#anscombeplot){reference-type="ref"
reference="anscombeplot"}, we can revise the original simple linear
model system to handle these anomalies as follows:

1.  Read in data

2.  Fit model $y=\beta_0+\beta_1x+\varepsilon$ to the data.

3.  Fit the alternative model
    $y =\alpha_0+\alpha_1x + \alpha_2x^2+\varepsilon$ and output
    $\hat{\alpha}_2$. Our expectation is that $\alpha_1 > 0$ and
    $\alpha_2=0$ (i.e. a simple linear model). If
    $\frac{|\hat{\alpha}_2|}{\sqrt{\mbox{Var}(\hat{\alpha}_2)}} \leq 2$,
    then the algorithm continues. Otherwise, stop the
    algorithm.[\[step:quadratic\]]{#step:quadratic
    label="step:quadratic"}

4.  To handle outliers in the $x$-direction, we can compute the leverage
    $h_i$ for each $x_i$ value from the linear model. If
    $h_i \leq 4\bar{h}$ for all $i$ (where $\bar{h}$ is the mean of
    $h_i$), then continue.[\[step:xoutlier\]]{#step:xoutlier
    label="step:xoutlier"}

5.  To detect outliers in the $y$-direction we can compute the
    studentized residuals
    $e_i = (y_i-\hat{y}_i)/(\hat{\sigma}\sqrt{1-h_i})$ from the linear
    model. If $|e_i| \leq 3$ for all $i$, then
    continue.[\[step:youtlier\]]{#step:youtlier label="step:youtlier"}

6.  Output $\hat{\beta}_0$ and $\hat{\beta}_1$.

This revised algorithm (shown in
Figure [6](#modelSLRrev){reference-type="ref" reference="modelSLRrev"})
introduces three checking steps, each of which has the possibility of
stopping the algorithm before completion. The checking steps act as
switches---if they evaluate to true, then a a switch is connected
allowing the analysis to proceed to the next step. In order for the
output to be generated, all three switches in
Figure [6](#modelSLRrev){reference-type="ref" reference="modelSLRrev"}
must be closed. Of course, we have made explicit choices here in
Steps [\[step:quadratic\]](#step:quadratic){reference-type="ref"
reference="step:quadratic"}--[\[step:youtlier\]](#step:youtlier){reference-type="ref"
reference="step:youtlier"} that might have been different depending on
the circumstances of the analysis.

![Revised simple linear regression
system.](modelSLRrev.png){#modelSLRrev width="6in"}

One might recognize the three additional checks presented in
Steps [\[step:quadratic\]](#step:quadratic){reference-type="ref"
reference="step:quadratic"}--[\[step:youtlier\]](#step:youtlier){reference-type="ref"
reference="step:youtlier"} as common checks of the assumptions of the
linear model. Indeed, "checking the assumptions" for a linear regression
model can be thought of as a way to decrease the size of the anomaly
space for a statistical methods system while also ensuring that
erroneous or misleading results do not emerge from the system.
Alternatively, we can say that these checks increase the size of the
minimal cutsets for the fault trees in the anomaly space.

Consider the anomaly "$\hat{\beta}_1 < 0$ when outputted from the model
fit\" for the system in Figure [6](#modelSLRrev){reference-type="ref"
reference="modelSLRrev"}. The fault tree for the revised simple linear
regression system with additional checks is shown in
Figure [7](#ftreeSLRrev){reference-type="ref" reference="ftreeSLRrev"}.
The top of the fault tree for this anomaly class is similar to the one
shown in Figure [2](#ftreeSLR){reference-type="ref"
reference="ftreeSLR"}. However, when we take the branch that considers
"contaminated data\", things begin to change. We can see that the two
AND gates ($G_2$ and $G_5$) below the event "Contaminated data produce
slope $< 0$\" imply that multiple events have to occur in order to
trigger this type of anomaly.

![Revised fault tree for simple linear regression system with additional
checks.](ftreeSLRrev.png){#ftreeSLRrev width="5in"}

The minimal cutsets for this part of the fault tree are all of size 5,
meaning that five events have to occur in order to trigger the anomaly
at the top of the tree. For example, one of the cutsets indicates that
if contaminated data were to produce $\hat{\beta}_1<0$, we would require
that (1) outliers were present ($E_{11}$); and (2) the contamination was
arranged to produce $\hat{\beta}_1<0$ ($E_{12}$); and (3) the quadratic
coefficient in the quadratic model was close to 0 ($E_9$); and (4) all
residuals $|e_i|<3$ ($E_4$); and (5) all leverages $h_i<4\bar{h}$
($E_6$). While it is possible to imagine this scenario occurring as a
result of random fluctuations in the data, it is more likely that we
would observe this pattern because of a change in the process governing
the relationship between $x$ and $y$ (i.e. the other branch at the top
of the tree). In other words, with this revised statistical methods
system, if the anomaly at the top of the tree were observed to occur,
the more likely explanation is that our model is incorrect rather than
there being a problem with the data.

## Discussion {#sec:discussion}

Data analysis is often depicted as an iterative process, but the precise
details of how this iteration works have not been specified in a manner
that can be broadly generalized. This paper attempts to systematically
describe this iterative process by framing a critical aspect of data
analysis as a process involving the recognition of anomalies and the
enumeration of their root causes. The consideration of anomalies in data
analysis provides a mechanism for interpreting statistical output in a
manner that allows us to adjust our expectations about the data
generation process and revise the statistical methods systems we build.

Perhaps the most useful aspect of framing the data analytic process in
terms of anomalies is that it forces us to intentionally consider a set
of expected outcomes when a statistical methods system is applied to
data. This "pre-registration\" of outcomes is what allows us to detect
anomalies in the first place and gives us a concrete basis for
interpreting the output from an analysis. While experienced analysts
likely already do this (perhaps unconsciously) and can iterate through a
series of anomalies and root causes quickly, having an explicit
statement of expected outcomes and a formal methodology for identifying
root causes may be valuable to novices who are still learning the craft.
Some have suggested that the reliance on explicit hypotheses can lead
analysts to overlook important aspects of the
data [@yanai2020hypothesis]. However, we would argue that it is not the
pre-specification of outcomes that is the issue here, but rather the
choice of confirmatory methods systems in situations where the data
generating process is not well-understood.

We have proposed the use of fault trees as a formal approach to
identifying the possible root causes of a data analytic anomaly. This
methodology is well-established in the systems engineering literature
and its general properties are described elsewhere [@michael2002fault].
However, we struggle to find any explicit use of fault trees in the
statistical analysis literature. We believe fault trees have something
to contribute to the process of doing data analysis. They force analysts
to consider the anomaly space of a statistical methods system and to
interrogate the various possible anomalies that the system may produce.
Fault trees provide a new approach to comparing the strengths and
weaknesses of different analytic systems in terms of how anomalies can
be identified and under what conditions they might occur. In particular,
they provide a way to characterize the differences between exploratory
and confirmatory statistical methods systems. Finally, fault trees can
record the historical knowledge of specific types of data analysis and
can serve as a useful educational tool for teaching newcomers to those
areas.

We hypothesize that the breadth and depth of a fault tree are directly
connected to the range of experience of the analyst or analysts who are
using the system. Experience can be gained over time or by working with
and learning from a wide range of collaborators. Diversity of
experience, both across time and across people, can only serve to
improve the quality and usefulness of the fault tree. Furthermore, the
fault tree can be passed down as a record of experience to new analysts
working with the same system or with similar data.

A topic not covered here is the question of how can analysts recognize
an anomaly in the first place? Anomaly *detection* requires having
enough background knowledge to create a reasonable set of expected
outcomes. If the set of expected outcomes is too large, then there will
be no chance of observing any anomalies and no way to inform our
expectations or update our analytic systems. The ability to construct a
proper set of expected outcomes can be reinforced through rigorous
preparation (e.g. summarizing the literature, meta-analysis),
pre-registration of hypotheses [@asendorpf2013recommendations], and
experience working with similar data.

Data analysis is a process that involves making a series of decisions
and the manner in which we make those decisions is often not
well-documented [@simmons2011false; @gelman2014statistical]. While fault
trees provide a formal mechanism for enumerating the possible root
causes of an anomaly, the analyst must then decide what to do next. For
example, the analyst must decide which collection of root causes should
be further investigated. This choice could be made based on the
analyst's determination of the relative likelihood of each of the root
causes, with the most likely cause being the highest priority. Given the
decision-making nature of data analysis, it may be possible to employ
the tools of decision theory to optimize the analyst's
choices [@smelser2001international]. However, any application of
decision theoretic tools would require a much more precise specification
of the decision process than is likely available. Notions of utility and
consequences may only be vaguely known and it may not be possible to
assign a probability distribution to the various root causes in the
fault tree. However, with experience, and perhaps after collection of
some data on decisions made in the past, it may be possible to employ a
formal decision-theoretic framework to the data analytic decision-making
process.

In this paper we have intentionally avoided discussion of software
implementations. Modern data analyses are necessarily implemented in
software, which can produce their own anomalies separately for reasons
that may or may not be related to the data analysis itself. For example,
a poorly formatted data file might cause software reading in that data
file to crash. Data analysts must to some extent be able to trace
anomalies or outright failures to possible software-related root causes.
Therefore, familiarity with software implementations may be of equal
importance to familiarity with the statistical properties of the methods
implemented. Our discussion of anomalies here parallels ideas in
software unit testing, which is a practice that is employed to ensure
that software anomalies are detected in the development
process [@runeson2006survey; @testthat2011].

Considering the root causes of data analytic anomalies reinforces the
importance of the reproducibility of analyses [@peng2011reproducible].
Reproducible research allows others to reconstruct the statistical
methods system that was applied to the data. In a "post-mortem\"
situation where an anomaly is being investigated, possibly by an
independent third party, having the code and datasets available is
critical for reconstructing the sequence of events that lead to the
anomaly. Without such information, it is impossible to construct the
fault tree necessary for enumerating the possible root causes.

It is interesting to consider the approach proposed here in the context
of modern machine learning algorithms. Iterative methods like boosting
essentially attempt to automate the evaluation of anomalies and update
their predictions successively based on pre-determined rules for
evaluating their fault trees. The original AdaBoost algorithm
re-weighted misclassified  observations more heavily so that successive
iterations would produce weak classifiers focused on those
values [@friedman2002stochastic; @friedman2000additive]. This implies
that in evaluating the anomaly of a misclassified  observation, AdaBoost
always takes the branch of the fault tree that considers the model to be
somehow incorrect. Many have pointed out that the performance of such
algorithms is degraded by outliers and have proposed robust
alternatives [@long2010random; @li2018boosting].

In this paper we focus on what can be learned from a single dataset in a
given data analysis. Our goal was to develop an approach that would be
useful to a data analyst at the moment of data analysis. However, there
clearly are limits on what can be learned from the observed data. As
presented here, some anomalies, such as outliers or nonlinear structure,
can be observed. But other problems, such as the presence of unobserved
confounders, that might affect the external validity of an analysis,
simply cannot be checked with the observed data [@ogburn2019comment].
Hence, we must draw a distinction between "assumptions\" that can be
checked and genuine assumptions that simply must be assumed to be true.
In general, checkable assumptions can be evaluated with the observed
data and any information gained can be incorporated into the statistical
methods system (as we demonstrated in
Section [5.4](#sec:slrfixed){reference-type="ref"
reference="sec:slrfixed"}).

The process of doing data analysis is complex and often idiosyncratic
but statisticians should not refrain from attempting to describe it
formally. As data science and statistics become more fundamental to many
aspects of society, we need to develop ways to generalize our approach
to analyzing data and translate those ideas to ever wider audiences. We
propose that characterizing data analysis as a process of diagnosing
anomalies and identifying root causes via fault trees is one such
generalizable approach that can serve as a guide in a wide variety of
data analyses.

Fault Tree Notation {#sec:notation}
===================

Comprehensive details on constructing fault trees can be found in
Vesely, et al. [@vesely1981fault] and Ericson [@ericson2011fault]. We
provide an abbreviated summary here that is sufficient for understanding
the fault trees in this paper.

If we consider the system shown in
Figure [1](#modelSLR){reference-type="ref" reference="modelSLR"}, we
could implement it with the following R code.

    ## Read in data from CSV file
    dat <- read.csv("dataset.csv") 

    ## Fit linear model
    fit <- lm(y ~ x, data = dat)

    ## Extract estimated coefficients
    b <- coef(fit)

    ## Return `b'

If we assume that the code underlying the execution of the system works
as expected and that the data are properly formatted and clean, then the
code will execute and coefficient vector `b` will be returned. If we are
to consider what are the failure modes of this system, it must be noted
that the coefficient vector `b` is the *only* output from the system
according to Figure [1](#modelSLR){reference-type="ref"
reference="modelSLR"}. Therefore, any failure modes associated with the
system must correspond to the coefficient values.

