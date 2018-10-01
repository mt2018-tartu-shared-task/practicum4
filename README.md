# practicum4

# Homework

## System evaluation and error analysis
```
NB1: this task should be done in a team. You can divide responsibilities or do the task jointly.
```

```
NB2: if you have any questions about the homework or course, open a ```Github issue``` at the your team's homework's repo. Use email only in case of individual questions you do not want your team to see.
```

### System evaluation
At the seminar we had a troubleshooting session.

For this homework your first task is to take your correctly trained baseline system and translate your dev set with it. Name the output file ```hyps.baseline.raw.en```. Then postprocess it and name result file ```hyps.baseline.en```. Compute BLEU score with the ```beam size parameter equals 8``` on the postprocessed file. Report resulting score and upload both files to your teams' github repo created for this homework.

As a reminder (and to help a bit these who did not come at the seminar) here are couple of points:
1. Make sure you have a properly trained baseline. If you did not manage get it by this week's deadline, you should have it trained by the next deadline anyway. Common mistakes include not preprocessing dev set with new truecasing and BPE models, not using all data for the training set, using 3000 insted of 32000 BPE units, etc.
2. Do not forget to postprocess your translated dev set before computing BLEU
3. When you translate, doublecheck you sbatch settings.
4. You can try to either translate on CPU with flag --use-cpu or on GPU
5. Do not forget to ask for a correct node for the point ``3``.
6. Not using correct config parameters for training
7. etc

### Error analysis
Assumes you have done the part 1 and got reasonable BLEU score (about 20).

If you did not mange to train your baseline properly, open a Github issue.

Write a report about the quality of the machine translation.
Go over at least 40 sentences (60 for 3-person teams) from the dev set, manually inspect each sentence,
and report for each sentence:
1. sentence id (line number)
2. the source sentence
3. reference translation
4. the machine translation
5. an assessment of the error in the machine translation
You may do step 5 in any way you want. For instance, you could classify errors as
“reordering errors”, “untranslated words in source”, or any other type of error you can
think of. Use also “it tried to represent meaning, but made grammatical errors” or
“hyps is fluent, but does not represent meaning correctly”,

For instance:
1. ID: 1984
2. Erst drei Tage ist der neue Ministerpräsident Griechenlands im Amt.
3. The new Prime Minister of Greece has been in office for only three days
4. Only three days is the new Prime Minister of Greece in office.
5. (1) Verb tense is wrong: is instead of has been. (2) Preposition for was missing
in front of the time phrase only three days. (3) While the noun phrases and
preprositional phrases are correct, the overall sentence structure on the clause
level is scambled.
(adapted from http://mt-class.org/jhu-2016/hw0.html):

Another example:
1. ID: 2019
2. kirjalikult . - ( IT ) Austatud juhataja , daamid ja härrad . mina hääletasin
härra Gyürki riiklikke energiasäästu tegevuskavasid järelkajastava raporti
poolt .
3. in writing . - ( IT ) Madam President , ladies and gentlemen , I voted in favour
of Mr Gyürk 's report on follow-up of the energy efficiency national action
plans .
4. in writing . - ( IT ) Madam President , ladies and gentlemen , I voted in favour
of Mr Beaupuy 's report on the national energy saving action plans .
5. Good translation with perfect fluency. (1). But minor adequacy problems:
guessing the "Madam" President from the genderless Estonian input wrongly
(2). The name "Gyürk" is translated as "Beaupuy" (segmented as Gy-ür-ki by
BPE) (3)

Obligatory include 5 last and 5 first sentences (as a part of the whole 40/60 sentences) from the dev set in your report.

Also take a loot at https://github.com/fishel/kama/ (open nmt and smt subfolders).
Conclude your report with a summary of your impression of the major quality
problems in the machine translation system that you analysed.

### Analyse attentions
Generate attention visuals for 6 (9 for teams of length 3) senteces from the ones analized above and write a 2-sentence explanation of weather what you see provides any additional info to analyze the translation errors. Also give your observation of weather it somewhat correlates with what you wrote about these translations in the previous section.

In order to generate attention visuals for sentences, you can either use sockeye default functionality for translation (add ```--output-type align_plot``` to your translate command, see also ```translate``` command docs for details) or https://github.com/M4t1ss/SoftAlignments (migh need to do ```sudo apt get php``` and ```export BROWSER=google-chrome``` if you are using chrome) that runs web server locally

Note that the alignment plot shows the subword units instead of tokens, as this is the representation used by Sockeye during translation. Additionally you can see the special end-of-sentence symbol </s> being added to the target sentence.
