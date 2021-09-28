<h1><p style="color:#DC267F"><b>Automated Error Analysis for Document Level Information Extraction From Scientific Text</b></p></h1>

<img src = "assets/error_analysis_system.png" height = "350" width = "900"/>
Figure 1: The document-level extraction task (left) and the automatic error analysis process (right).

<h3><p style="color:#FE6100"><b>Introduction</b></p></h3>

<p style="color:black">
Recent works proposing modern NLP models for the document-level information extraction tasks (Du et al., 2021; Li et al., 2021; Jain et al., 2020) have their performance measured by different datasets via varying metrics, with <span style="color:#785EF0">limited, often manual analysis of model errors which are less informative</span> (e.g., scores alone cannot detail model’s tendency to assign incorrect types to selected noun phrases, or frequency of spurious templates/clustering generation). <span style="color:#785EF0">There has not been a systematic investigation on models’ error profiles across domain-diverse document-level IE datasets.</span></p>

<p style="color:black">
In this work, we introduce several representative document-level IE datasets. Then, inspired by the work of Kummerfeld and Klein (2013) on tackling the error analysis challenge for coreference resolution, we introduce a framework for <span style="color:#785EF0">automating the process of error analysis</span> for document-level information extraction tasks. This framework automatically <span style="color:#785EF0">transforms the predicted outputs into the desired extraction structures</span>, through a set of transformations (Figure 1), and then <span style="color:#785EF0">maps each transformation or a set of transformations</span> into sensible and linguistically motivated <span style="color:#785EF0">error types</span>(Table 1). We apply two state-of-the-art neural methods to three document-level information extraction tasks from multiple domains including the scientific literature. One method (i.e., GTT (Du et al., 2021) is designed for doc-level extraction; the other is DyGIE++ (Wadden et al., 2019), sentence-level extraction augmented with clustering to create the doc-level representation. We also replace their base pre-trained model with SciBERT (Beltagy et al., 2019). We analyse their behaviors and errors in their predictions via our automatic framework.</p>

<h3><p style="color:#FE6100"><b>Datasets</b></p></h3>
