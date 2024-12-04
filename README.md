java c
BMEN90035   Biosignal   Processing 
Final   Exam, Semester   2, 2024
Number of questions: 2                            Total Marks: 120 (1 mark per minute of writing time) 
• Question 1: 50 marks 
• Question 2: 70 marks 
Time allowed: 
• 40 minutes reading time - you MAY begin writing during this time 
• 120 minutes writing time 
• 30 minutes upload time - you must NOT write during this time 
The exam is openbook. You may use code from Class Exercises and Workshops as templates to modify to complete this exam. You may also consult resources on the internet, excluding artificial intelligence tools, such a Chat GPT or Wolfram Alpha. Additionally, a table of Fourier transforms available for download from the LMS Assignments page for this exam may be useful. 
A copy of this exam is available in pdf format from the LMS Assignments page in case any equations are difficult to read in the .mlx file. 
You may NOT share code or answers with other students during the exam. 
Data provided 
The following data are   provided to be used   in   answering ALL Questions:• e1, e2   - two   Matlab variables corresponding to two electromyogram      (EMG) envelope   signals   elnl      and  ,    respectively,   in units of volts. The two   EMG signals were   recorded   simultaneously   from   different   electrodes, on channels   1 and 2 (respectively), on   the forearm   of   a   subject   while   typing. •   f_s - a   Matlab variable corresponding the sampling frequency f   =   1000   Hz   used for   elnl    and   eal   ml .•   cmap   - a   Matlab variable corresponding to a colour   map that   is   useful for   Q1f,   Q2c,   Q2d   and   Q2g.   It   is   a   4x3   matrix, where   rows   1 to 4 correspond to the colours cyan,   blue, green,   and   red,   respectively. 
Please   note that answers to some questions are   used   in subsequent   questions   as   part   of a   multi-step   signal processing   pipeline. Consequently, the following supplementary variables are provided to   allow   you   to   complete   these subsequent questions   in case you are unable to   calculate   these   variables   in   the   original   question.   These         variables are all stored   in a   Matlab   structure   soln. 
• soln.P1_volts and    soln.P2_volts   - corresponding to   key-press   segments   p1,   m[k]    and p2,   m[k]   calculated   in Q1b. These variables are stored in the   1000x80   matrices,   pi   ,   as   described   in   Q1b. 
•   soln.p1_bar_volts and    soln.p2_bar_volts - corresponding to the   mean   key-press   segments, and    , calculated in Q1c. These variables   are   1000x1    vectors,  .
• soln.Q1_volts and    soln.Q2_volts   - corresponding to   mean-subtracted   key-press   segments:      q1,   m[k]   and   q2,   m[k] , calculated   in Q1c. These variables are stored   in    1000x80   matrices,  ,    in   the   same   format   to the matrices   pi   described   in   Q1b. 
•   soln.h1 and    soln.h2   - corresponding to first two   principal   components,  and  ,   calculated   in   Q1d.
• soln.ym1_volts and    soln.ym2_volts   - corresponding the   projections   Y1,m   and   )2m   calculated   in   Q1f.   These variables are in   1x80 vectors giving the   projections to the   key-presses m=1,.,80   . 
• soln.y1_volts and    soln.y2_volts - corresponding   to   filter   outputs   y[n]    and  calculated   in   Q2c. 
• soln.y1_3rd_volts and    soln.y2_3rd_volts   - corresponding to filter outputs  and  for the   third   key   press of each finger, as calculated in   Q2d.   These variables   are   1000x4   matrices, where   each   column corresponds to   a key-press for fingers   1 to 4,   respectively. 
•   soln.r_volts   - corresponding to the distance,   r[n] , calculated   in Q2e.
These variables can be loaded into   your workspace   by   running   the   following   lines   of code.
clear all 
load ExamData2024.mat 
Overview 
Electromyograms (EMG) measure the electrical activity of   muscles   during   their   contraction   using   electrodes placed on the surface of the skin.   Most useful   information   in the   EMG   appears   in   the   envelope   of the   signal   (this   is a   positive,   relatively slow signal that   modulates a faster,   noisy carrier signal).   In   this   exam   you will   analyse EMG envelope signals obtained from recordings made   during   typing,   for   potential   application   in   control   of a robotic   hand for typing. These   EMG envelopes,   eu[n]    and  , were obtained from simultaneous   recordings   on channels   1 and 2 (respectively), using from   electrodes   at   different   locations   on   the   forearm.   During   the recordings, the subject   pressed   keys on a   keyboard with one finger at a   time.   There   were   80   key   presses   in   all,   made at a   rate of   1   press   per second   as follows:
• key   presses   1-20: finger   1 =   index finger,
• key   presses 21-40: finger 2 =   middle finger,
• key   presses 41-60: finger 3 =   ring finger,
• key   presses 61-80: finger 4 =   little finger.
The goal of the application   is to   use the   EMG envelopes,  and  , to determine   when   a   key   press   has   taken   place and which finger (1 to 4) this corresponds   to.
This will   be done   in two stages:
• Question 1:   Performing an off-line analysis using   principal components analysis   of the   EMG   envelopes.   This will   identify a two-dimensional   projection space   in which   it   is   possible to classify which finger was               used to   press the   key for each segment of the   EMG envelope.
• Question 2:   Designing and   implementing an on-line   processing algorithm to determine when   a   key-press   has taken   place and which finger (1 to 4) was used.   This   is   based   on   the   principal   components   analysis            from Question   1.
Question 1: Performing an off-line analysis using   principal components analysis   of the   EMG   envelopes.   This            will   identify a two-dimensional   projection space   in which   it   is   possible to classify which finger was used   to   press   the   key for each segment of the   EMG envelope.
Q1a. [4 marks] Figure   1:    Plot the two   EMG   envelopes,   elnl    and   e   a[n] , over the duration   of the   recording.   Plot   elnl   in   subplot(2,1,1)   and   ea[n]   in   subplot(2,1,2).   Label your axes showing time   in seconds and signal            amplitude   in volts. Give the figure a title of Fig.   1.   (Note   here   and   throughout   the   exam,   when   giving   a   title   to   a         figure with subplots, you may apply title to just the first subplot.)
%% Q1a code 
figure(1), clf subplot(2,1,1) plot(t,e1,'k') title('Fig.1)') 
subplot(2,1,2) 
plot(t,e2,'k') 
xlabel('time [s]') 
ylabel('amplitude [V]') 
Q1b. [6 marks] The first step   in   performing   principal components analysis   is to   partition   each   of the   envelopes   elnl   and   e   a[n]   into 80 one-second segments, each corresponding to a   key-press.   Call   these   key-press segments p1,   m[k]   and p2,   m[k] ,    respectively, form=1,.,80   key-presses, and k=1,…,1000   samples.
Extract each segment pj.   m[k] , for each electrode.   For the first segment of each   electrode   use   samples   n=   l to   1000 (corresponding to the   1st to   1000th samples of   eu[n]    and   eznl ); for the second segment   use   samples            n=   1001   up to 2000, and so on for the 80   key-presses.      (Hint:   consider   using the   Matlab   command   reshape to   produce a   1000x80   matrix,   pi   , for each electrode with pi.   m[k] , k=1,…,1000   , as the columns). 
In   Figure 2    plot all 80   key-press   segments   overlaid,   plotting those   for   the   first   electrode,   p1,   m[n] ,   in subplot(2,1,1) and those for the second electrode, p2,   m[n] ,   in   subplot(2,1,2).   Label your axes   showing   time   in seconds and signal amplitude   in volts. Give the figure   a   title   of   Fig.   2.%% Q1b codefigure(2), clf
Q1c. [8 marks] The   next step   in   performing   principal components analysis   is to   calculate the   mean   key-press   segments,    and    , over the 80   key-presses for electrodes   1   and 2,   respectively. Then   use   these   to
calculate the   mean-subtracted   key-press segments:       and    ,   for   each key-press m=1,.,80   .
Calculate the   mean-subtracted   key-press segments:    q1,   m[k]    and   q2,   m[k] , m=1,.,80   .   Plot them overlaid   in
Figure 3,   plotting those for the first electrode,   q1,   m[k] ,   in   subplot(2,1,1)   and those for the   second   electrode,   q2,   m[k] ,   in   subplot(2,1,2).   Label your axes showing time   in seconds and signal   amplitude   in   volts.   Give   the   figure a title   of   Fig.   3.
(Note:   if you were   not able to   calculate the   key-press segments p1,   m[k]    and p2,   m[k]       you   may   use a version
stored in   soln.P1_volts and    soln.P2_volts   to complete this   question.   These variables   are   the   1000x80   matrices,   pi   , described   in Q1a.)
%% Q1c code 
figure(3), clf 
Q1d. [10 marks] Perform. principal component   analysis   on   the combined mean-subtracted   key-press
segments. To combine them, concatenate the segments together for channels   1   and   2    for   each   key-press,
m.   I.e.   if    1,m   and   q2m   are the column vectors   corresponding to   segments   q1,   m[k]    and   q2,   m[k] ,   respectively,   make   a vector of twice their   length   by appending   92m   under   1,m   :      qm=Iq1,   m;q2,   m] .
Plot the first two principal components,   hi    and   hc   in   Figure 4, using sample   numbers for the timeaxis.   Label            your axes showing time   in samples and component amplitude   (unitless).   Give the   figure   a   legend   and   a   title   of   Fig. 4.
(Note:   if you were   not able to calculate the   mean-subtracted   key-press segments,      q1,   m[k]    and   q2,   m[k] ,    you
may use a version stored in   soln.Q1_volts and      soln.Q2_volts   to   complete   this   question.   These   variables   are   1000x80   matrices,  ,    in the same format to the matrices   pi   described   in   Q1b.)
%% Q1d code 
figure(4), clf 
Q1e. [6 marks] Interpret the two   principal components   in   terms   of the   relative   amplitude   and   polarity   of the 代 写BMEN90035 Biosignal Processing Final Exam, Semester 2, 2024Matlab
代做程序编程语言  signals on each of the two   electrodes.
(Note:   if you were   not able to calculate the first two   principal components,  and  ,    you   may   use   a version   stored   in   soln.h1 and    soln.h2 to complete this   question.)
Q1f. [8 marks] Project the segments   qm   onto the   2-dimensional   projection   space   defined   by the   first   two
principal components  and  :   i.e. onto   points   (ym,yz   m)=(hf   qm,   h⃞qm)            (Eq.1)
Plot the   results   in   Figure 5 as a scatterplot of   2m   versus   Y1,m   .   Colour   code   the   points   in   the   figure   so   that   "finger   1" = cyan, "finger 2" =   blue,   "finger   3"   =   green   and   "finger 4"   =   red.   (The   colour   map   cmap   is   provided   for you.
It   is a   4x3   matrix, where   rows   1 to 4 correspond to the colours   cyan,   blue,   green,   and   red,   respectively.)   Label   your axes,   including showing the   projection amplitude scale   in volts. Give the figure   a   title   of   Fig.   5.
(Note:   if you were   not able to calculate the first two   principal components,  and  ,    you   may   use   a version
stored   in   soln.h1 and    soln.h2 to complete this   question.   Similarly,   if you were   not   able to   calculate   the
mean-subtracted   key-press segments:    q1,   m[k]    and   q2,   m[k] ,    you   may   use a version   stored   in   soln.Q1_volts
and    soln.Q2_volts   to complete this question. These variables   are   1000x80   matrices,   Q;   ,      in the   same format   to the matrices   pi   described   in   Q1b.)
%% Q1f code 
figure(5), clf 
Q1g. [8 marks] Propose a criteria   based on   the   projections   y1,m    and   Y2,m    in   Figure 5   to   classify   each   key-press   as corresponding to one of the fingers   1 to 4 (the   criteria   need   not   have   perfect   accuracy).      Explain   the   merits            and deficiencies of your   criteria.
(Note:   If you were   not able to calculate the   projections   Y1,m    and   Y2,m   , you   may   use a version   stored   in
soln.ym1_volts   and      soln.ym2_volts   to   complete   this   question. These   variables   are   1x80   vectors   giving the   projections   to   the   key-presses   m=1,.,80).Question 2.   Designing and   implementing an on-line   processing algorithm to determine when   a   key-press   has      taken   place and which finger (1 to 4) this corresponds to. This   is   based   on   principal   components   analysis   from   Question   1.
Consider the decomposition of the first   principal component,   h1=[h11;h12] ,    where
• hn   is the subcomponent of hi    corresponding   to signals from electrode   1, and
•  is the subcomponent of  corresponding to signals from electrode   2.
Similarly for the second   principal component,   h2=   [h21;h22] , where
•  is the subcomponent   of  corresponding to signals from electrode   1,   and
• h22   is the subcomponent of hc   corresponding to signals from   electrode   2.
Q2a. [14 marks] [This question requires handwritten answers to be scanned and uploaded]. Considering,   applying   these   subcomponents,   hj   ,    as   templates   for   matched   filters,   sol   kl      (k=0,…,999), to   their
corresponding envelopes   eynl   so as to correctly   transform. them into   the   principal   component   projection   space   (y[n],   ya[n])   defined by   Eq.1 in Q1f. That is,   at   each   time   step, n,   xnl    is   calculated   by applying             the steps of mean subtraction and projection   described   in   Q1b,   Q1c   and   Q1f to the   two   vectors
e,in   l=le,   in-999),e,in-998],…e,   in   ll', (j=1,2) that contain the last   1000 samples of   einl   .   Show   that
(y[n],   ya[n])   are given   by
y[n]=(g11xe)[n]+(g12xe)[n]-h   fp1-hp2             (Eq.2)
(Eq.3)
where  (j=1,2) are the column vectors corresponding to the mean   key-press   segments,
.
Q2b. [10 marks] [This question requires handwritten answers to be scanned and uploaded]. Consider the   pair   of signals:
• dI[n]=hu[n]   for   n=0,…,999   ,      and   d[n]=0   otherwise, on   channel   1,
• dz[n]=h2[n]   for   n=0,…,999   ,    and   d[n]=0   otherwise, on   channel   2.
That is,dnl is the subcomponent of the first principal component   corresponding   to   channel j,   starting   at   time   zero.
1.   Prove that the value of   delay,   na   , from the onset of the signal at   n=0   that   will   lead   to   a   maximal   output   y[n]   for   the   matched-filter   is   n   a=999   samples?
2.   Theoretically, what will the value of  be at this delay time, ?   Explain   your   answer.
Q2c. [8 marks] Apply the formulae   in   Eqs. 2      3 to   calculate   the filter   outputs   (y[n],   ya[n])   .   Plot   the   trajectories   of  versus  in the   principal component   projection space,   in   Figure 6.
Colour-code the trace so that those   parts of the signal corresponding fingers   1,   2,   3   and 4   are   plotted   in   cyan,   blue, green and   red,   respectively.    In doing   so,   account for the   delay,   na   ,      due   to   the   matched   filter   (defined
in   Eqs. 2     3) that would   be expected   to   result   in   an   optimal   match   to   the   key-press   segments, p1,   m[k]    and
p2,   m[k] , defined   in Q1b.   (Note: The colour map   cmap   is   provided for you.   It   is   a   4x3    matrix, where   rows   1   to   4   correspond to    the colours cyan, blue, green,   and   red,   respectively.)
Label your axes showing the   projection amplitude   in volts. Give the figure   a   legend   and   a   title   of   Fig.   6.
(Note:    if you were not able to calculate the   envelopes   elnl    and   eznl    you may use a   version   stored   in
soln.e1_volts and    soln.e2_volts   to complete this question.   Similarly,   if you were   not able   to   calculate   the         first two   principal components,   hi    and   hc   ,    you   may   use a version stored   in   soln.h1 and      soln.h2 to complete   this question. Similarly,   if you were   not able to calculate the   mean   key-press segments,       and    ,      you
may use a version stored in   soln.p1_bar_volts and      soln.p2_bar_volts   to   complete   this   question.   These   variables are   1000x1   vectors, .)
%% Q2c code 
figure(6), clf 
Q2d. [8 marks] Using your calculation of the filter outputs   (y   ln],   ya[n])   ,   plot the trajectories of  versus  for   just the third key-press for each finger,   1 to 4. To do this   use   the   segment   of the   signal   (y[n],   ya[n])   that:
•   corresponds to 500   ms   before the third   key-press was   initiated, to 500   ms after the third   key-press   was   initiated, and
• accounts for the delay,   na   ,    due to the   matched filter (defined   in   Eqs.   2         3)   that would   be   expected   to   result   in an optimal   match   to the   key-press segments, p1,   m[k]    and p2,   m[k] ,   defined   in   Q1b.
Once again, colour-code the trace for each finger   1, 2, 3   and   4   so   they   are   plotted   with   cyan,   blue,   green   and
red,   respectively. (The colour map   cmap   is   provided for you.   It   is a   4x3   matrix, where   rows   1   to   4   correspond   to   the colours cyan,   blue, green, and   red,   respectively.)
Label your axes showing   projection amplitude   in volts. Give the figure   a   legend   and   a title   of   Fig.   7.
(Note:    if you were   not able to calculate the envelopes  and  you   may   use   a version   stored   in
soln.e1_volts and    soln.e2_volts   to complete this question.   Similarly,   if you were   not able   to   calculate   the         first two   principal components,  and  ,    you   may   use a version stored   in   soln.h1 and      soln.h2 to   complete   this question. Similarly,   if you were   not able to calculate the   mean   key-press segments,       and    ,      you
may use a version stored in   soln.p1_bar_volts and      soln.p2_bar_volts   to   complete   this   question.   These   variables are   1000x1   vectors, .)
%% Q2d code 
figure(7), clf 
Q2e. [8 marks] Identify   by sight the approximate   "return"   position   (yr1,   yr,.2)   ,      evident   in   Figure   7;   i.e. the
approximate   point that the trajectory   (y[n],   ya[n])   returns to between each   key-press.   In   Figure 8   plot the
distance,   r[n] ,    of (y[n],   yz[n])   from the "return"   position   (yr,1,yr,2)   , as a   function   of time.   (In   your   answer,   specify   the "return"   position   by giving values   (yr,1,   yr:2)   to a 2-dimensional   Matlab variable y_r in   your   code).
(Note:   if you were   not able to calculate the segments of  and  corresponding to   the   third   key-press
in Q2d, you   may use a version stored   in   soln.y1_3rd_volts and      soln.y2_3rd_volts   to   complete   this
question. These variables are   1000x4   matrices, where each column corresponds to a   key-press   for fingers   1   to   4,   respectively.   )
%% Q2e code 
figure(8), clf 
Q2f. [8 marks] Propose (in words) a criterion   based on   the   distance,   r[n] , for first   identifying when   a   key-press   occurred, regardless of which finger was used. Justify your answer   using   Figures 6,   7   and   8.
(Note:   if you were   not able to calculate   r[n]    in Q2e, you   may use a   version   stored   in   soln.r_volts) Answer:
Q2g. [14 marks] Based on your answers to Q1gand Q2f devise   a   method   to   determine from   the   EMG
envelope data when a key-press has taken   place   and which finger   (1   to   4)   this   corresponds   to. Apply   your   method to the data   in envelopes  and  .Report the   results   by first   plotting the two   EMG envelopes,   elnj   and   elnl   ,   in   black over the duration of the      recording   in   Figure 9   in   subplot(2,1,1)   and   subplot(2,1,2),   respectively. Then   plot on top an asterisk         (*) along the timeaxis of both subplots at the   estimated time   of each   key-press from   Q2f.   Colour   code   each   asterisk with   black,   blue, green or red, corresponding fingers   1, 2,   3   and 4,   respectively.
Label your axes showing time   in seconds and envelope amplitude   in   volts.   Give   the   figure   a   title   of   Fig.   9.%% Q2g codefigure(9), clf
Total marks: 120 



         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
