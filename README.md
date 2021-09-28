<h1><p style="color:#DC267F"><b>Automated Error Analysis for Document Level Information Extraction From Scientific Text</b></p></h1>

<br>
<img src = "assets/error_analysis_system.png"/>

<br>
Figure 1: The document-level extraction task (left) and the automatic error analysis process (right).

<h3><p style="color:#FE6100"><b>Introduction</b></p></h3>

<p style="color:black">
Recent works proposing modern NLP models for the document-level information extraction tasks (Du et al., 2021; Li et al., 2021; Jain et al., 2020) have their performance measured by different datasets via varying metrics, with <span style="color:#785EF0">limited, often manual analysis of model errors which are less informative</span> (e.g., scores alone cannot detail model’s tendency to assign incorrect types to selected noun phrases, or frequency of spurious templates/clustering generation). <span style="color:#785EF0">There has not been a systematic investigation on models’ error profiles across domain-diverse document-level IE datasets.</span></p>

<p style="color:black">
In this work, we introduce several representative document-level IE datasets. Then, inspired by the work of Kummerfeld and Klein (2013) on tackling the error analysis challenge for coreference resolution, we introduce a framework for <span style="color:#785EF0">automating the process of error analysis</span> for document-level information extraction tasks. This framework automatically <span style="color:#785EF0">transforms the predicted outputs into the desired extraction structures</span>, through a set of transformations (Figure 1), and then <span style="color:#785EF0">maps each transformation or a set of transformations</span> into sensible and linguistically motivated <span style="color:#785EF0">error types</span> (Table 2). We apply two state-of-the-art neural methods to three document-level information extraction tasks from multiple domains including the scientific literature. One method (i.e., GTT (Du et al., 2021) is designed for doc-level extraction; the other is DyGIE++ (Wadden et al., 2019), sentence-level extraction augmented with clustering to create the doc-level representation. We also replace their base pre-trained model with SciBERT (Beltagy et al., 2019). We analyse their behaviors and errors in their predictions via our automatic framework.</p>

<h3><p style="color:#FE6100"><b>Datasets</b></p></h3>

<img src = "assets/dataset_stats.png"/>

<br>
Table 1: Dataset Statistics. A relevant document has one or more templates.

Table 1 presents statistics for the three datasets that we conduct experiments and error analysis on. 
- <span style="color:#785EF0">MUC-4/span> (MUC-4, 1992) consists of news, each of which describes one or more terrorist incidents. The slots/roles that we chose to extract from the dataset were *Incident Type*, *Perpetrator (Individual)*, *Perpetrator (Organization)*, *(Physical) Target*, *Victim* and *Weapon*.
- <span style="color:#785EF0">ProMed</span> (http://www.promedmail.org) consists of global disease outbreak reports. The slots/roles that we chose to extract from the dataset were *Status*, *Country*, *Disease*, and *Victims*.
- <span style="color:#785EF0">SciREX</span> (Jain et al., 2020) is a recent dataset for scientific document-level IE, consisting of annotated articles on Machine Learning from Papers with Code, where we specifically focus on its 4-ary relation extraction sub-task. The slots/roles that we chose to extract are *Material*, *Method*, *Metric* and *Task*.

<h3><p style="color:#FE6100"><b>Evaluation</b></p></h3>

<img src = "assets/f1eq_white.png">

We use <span style="color:#785EF0">F1 score</span> from MUC-4 (Chinchor, 1992) which is shown above, where Precision is a measure of the fraction of predicted role fillers that are correct, while Recall is a measure of the fraction of expected role fillers that are correctly predicted. As the Precision and Recall vary by how each predicted template is individually unmatched or matched with one gold template, we enumerate F1 scores for all possible mappings and use the highest F1 score matching to serve as a guide to our transformation and error analysis process.

<h3><p style="color:#FE6100"><b>Methodology</b></p></h3>

Similar  to  the  work  of  Kummerfeld  and  Klein (2013), our error analysis tool is  <span style="color:#785EF0">system-agnostic</span>, i.e. it only uses system output and does not consider intermediate system decisions. This is because we wanted a tool that would allow for error analysis and comparison across different kinds of systems - end to end or pipeline; neural or pattern-based, and there were no other methods that could be uniformly applied across all systems. Our tool is also  <span style="color:#785EF0">dataset-agnostic</span>, simply requiring the names of the slots/roles to be specified in order to read data correctly from the input file and displaying per slot precision, recall and F1.

Given the input consisting of the predicted and gold templates for every document in the dataset, our error analysis tool performs the folowing steps:

1. Finds an <span style="color:#785EF0">optimized matching</span> of the predicted templates to the gold templates per document, choosing the matching attains a higher total F1 score across all slots/roles.
2. Compares the matched pair of predicted and gold templates in a document, and <span style="color:#785EF0">applies transformations</span> in order to <span style="color:#785EF0">convert the system predicted templates into the desired templates</span> that would attain a 100% F1 score upon evaluation with respect to the gold templates. 
3. <span style="color:#785EF0">Maps the changes</span> made in the conversion process <span style="color:#785EF0">to different error types</span>.

<h3><p style="color:#FE6100"><b>Transformations</b></p></h3>

There are a fixed set of transformations involved in changing  the  predicted  templates  to  the  desired templates (with 100% F1) as detailed below:

  1. <h6><span style="color:#785EF0">Alter Span</span></h6> transforms a role filler into a gold role filler which has the lowest <span style="color:#785EF0">span comparison score (<i>SCS</i>)</span>. To calculate the span comparison score between two spans x and y, we use one of two scoring modes:
  - **absolute**:  This mode captures the (positive) distance between the starting indices (and ending indices) of spans x and y in the document, and scales that value by the sum of the lengths of x and y, capping it at a maximum of 1.
  
  <img src = "assets/scsabs.png"/>
  
  - **geometric_mean**: This mode captures the degree of disjointedness between spans x and y by dividing the length of the overlap between the two spans with respective to each of their lengths, multiplying those two fractions and subtracting the final result from 1. If *si* is the length of the intersection of spans x and y, and neither x nor y have length 0, *SCS* is calculated as shown below. Else, *SCS* is 1.

  <img src = "assets/scsgeo.png"/>
  
  Thus, if the predicted role filler is an exact match for the gold role filler, the *SCS* is 0. If there is some overlap between the spans, the *SCS* is between 0 and 1 (not inclusive), and if there is no overlap between the spans, the *SCS* is 1.

<h3><p style="color:#FE6100"><b>Error Type Mappings</b></p></h3>

<h3><p style="color:#FE6100"><b>Results and Analysis</b></p></h3>
