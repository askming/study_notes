# Trimmed means analysis

- In clinical trials, some patients may discontinue the randomized treatment due to lack of efficacy or toxicity, which can be considered as intercurrent event (ICE) under the ICH E9 framework. To handle such ICE, one can use the composite strategy, whereby one somehow incorporates the occurrence of the ICE into the definition of the outcome or endpoint variable.
  - when the outcome is binary, this approach is relative straightforward, for those experienced an ICE due to the above reasons, their endpoint value is set to the category or level which constitutes a bad outcome. Hence we lump together patients who experienced the intercurrent event with those that did not but which had a bad outcome nevertheless.
  - When the endpoint is continuous, applying the composite strategy is harder, i.e. whcih value should be assign for those patients with ICE?
    -  {cite:p}`permutt2017trimmed` proposed a trimmed means approach to answering this question. 
       -  Patients who experience the ICE are assigned a very bad value (e.g. the worst value possible for the variable) for the outcome, with the justification that the ICE occurring constitutes a bad outcome (in a general sense) for the patient. It does not matter precisely what value is assigned, because in the next step we trim (delete) the worst $x\%$ of patients from treatment group. 
       -  Thus as long as the very bad value is so bad that we are guaranteed the patients assigned this value are trimmed (excluded), it doesn’t matter precisely what the value is. We then calculate the difference in means (difference in trimmed means) between the two treatment groups.





```{bibliography}
:filter: docname in docnames
```

