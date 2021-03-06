# QuasarSelection

Repository for exploring machine learning algorithms for quasar
selection/classification.  These are intended for analysis of
SDSS+SpIES data, but can serve as an introduction for analysis of
other data sets (e.g., DES, LSST, etc.) for those looking for a
template to follow.  These are largely "sand boxes", so apologies if
they aren't fully coherent.  Though I have gone through them to try to
make sure that they have some longer-term value.

`SpIESHighzCandidateSelection.ipynb`<br> 
is a notebook that takes the training sets that we used for KDE
selection of 3.5<z<5 quasars from SDSS+SpIES data and does color-based
self classifation using other machine learning algorithsm from
Scikit-Learn.  KDE was about 78% complete and 97% efficient.  The
sklearn algorithms were about the same, which gives confidence that
the sample is not *highly* contaminated (which isn't to so that it
isn't contaminated at *all*) and that we could replace our usage of
the proprietary KDE algorithm with public algorithms from sklearn.  I
looked at neural networks, SVM, random forests, bagging, and boosting.
While bagging has the highest efficiency, it also takes considerably
longer than random forests, which provided the best overall solution
(in terms of completeness, efficiency and computing time).  The
results of this get applied to the test set in
`SpIESHighzQuasars.ipynb`.

`SpIESHighzCandidateSelection2.ipynb`<br> 
is similar to the notebook above, but here I was exploring what would
happen if we added i-band magnitude and u-band extinction as
classification attributes.  I also explored some different
combinations of selection algorithms.  I found that bagging including
i and extinctu has less contamin than anything else.
`SpIESHighzQuasars2.ipynb` is the notebook where I explore these
changes in selection.

`SpIESHighzQuasars.ipynb`<br>
is a notebook that applies the RF classifier from
`SpIESHighzCandidateSelection.ipynb` to the test set.  This uses
colors only.  I also explored with bagging, but that crashes and later
led to trying just to classify Stripe 82 objects with stand-alone
python code, running on the server.  See `SpIESHighzQuasarsS82all.py`.
But this notebook *does* successfully produce output classifications
for the test set using the RF classifier.  The output is in
`GTR-ADM-QSO-ir_good_test_2016_out.fits`.

`SpIESHighzQuasars.ipynb`<br>
is a notebook that applies the SVM and bagging classifiers from
`SpIESHighzCandidateSelection2.ipynb` to the test set.  I also tried
to create a new version of the test set that included i-band magnitude
and u-band extinction, but I ran into all sorts of problems.  (Though
it may be worth trying again).  In the end, I ended up writing a
stand-alone script `SpIESHighzQuasarsS82all.py` that just classifies
the objects in Stripe 82.  However, it does so using RF, SVM, *and*
bagging.  That way we can compare the results and find out which
produces the least contamination as that is important for our
clustering analysis.

`SpIESHighzQuasarsS82all.py`<br>
See note above.  Outputs `GTR-ADM-QSO-ir_good_test_2016_out_Stripe82all.fits`.

`SpIESHighzQuasarPhotoz.ipynb`<br>
The above notebooks just do selection, now we need to do photo-$z$ on
the output files with the quasar canddiates.  Here I play with
Nadaraya-Watson, Random Forest, Gaussian Process Regression, SVM, and
Stochastic Gradient Descent.  The first two are good, the next two
crash, and the last one is terrible.  Just doing self classification
of the training set here.

`SpIESHighzQuasarPhotoz2.ipynb`<br>
Now we apply the best photo-z algorithms to the test data,
specifically quasar candidates.  Right now just sticking to the Stripe
82 quasar candidates from
`GTR-ADM-QSO-ir_good_test_2016_out_Stripe82all.fits` as the
Nadaraya-Watson algorithm isn't particularly speedy.  Output is 
`GTR-ADM-QSO-ir_good_test_2016_out_Stripe82all_zphot.fits`.

Included quasar candidates file, which includes all of the test
object, not just the quasar candidates:<br>
`GTR-ADM-QSO-ir_good_test_2016_out_Stripe82all.fits.bz2`<br>
Actually, I couldn't because it exceeded the file size limit.  So ask me if you want it!

Included the quasar candidate file limited to quasar candidates only
with two photo-z outputs:<br>
`GTR-ADM-QSO-ir_good_test_2016_out_Stripe82all_zphot.fits`

This work was supported in part by NSF CDS&E grant 1411773.