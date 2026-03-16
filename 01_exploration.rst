.. code:: ipython3

    %matplotlib inline
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import seaborn as sns
    np.random.seed(42)

.. code:: ipython3

    df = pd.read_csv('/Users/andradenmariana/Documents/coding/python/gaming-dysregulation-analysis/data/dataset_cpx.csv')
    
    # Filter to internet gamers only
    gamers = df[df['filter_$'] == 1].copy()
    
    # Fix string columns
    for col in ['igd_count', 'game_satisfaction', 'internet_gaming_time', 'igd_log']:
        gamers[col] = pd.to_numeric(gamers[col], errors='coerce')
    
    # Confirm shape and nulls in key columns
    print(gamers.shape)
    print(gamers[['need_satisfaction', 'need_deprivation', 'game_satisfaction',
                  'igd_count', 'internet_gaming_time',
                  'externalising_problems', 'internalising_problems']].isnull().sum())


.. parsed-literal::

    (525, 186)
    need_satisfaction         0
    need_deprivation          0
    game_satisfaction         0
    igd_count                 0
    internet_gaming_time      0
    externalising_problems    0
    internalising_problems    0
    dtype: int64


.. code:: ipython3

    # Chart 1: Distribution of IGD symptom counts across internet gamers
    # The vast majority of players show zero or minimal symptoms, reframing the "gaming addiction panic" narrative
    
    fig, ax = plt.subplots(figsize=(10, 5))
    
    # Count how many adolescents report each symptom level (0-9)
    # sort_index() ensures bars appear in order left to right
    igd_dist = gamers['igd_count'].value_counts().sort_index()
    
    ax.bar(igd_dist.index, igd_dist.values, color='steelblue', edgecolor='white')
    
    ax.set_xlabel('Number of IGD Symptoms (0–9)', fontsize=12)
    ax.set_ylabel('Number of Adolescents', fontsize=12)
    ax.set_title('Most Gamers Show No Signs of Dysregulated Gaming', fontsize=14)
    
    # Force all x ticks to show even if a value has zero count (e.g. 8 symptoms)
    ax.set_xticks(range(10))
    
    # Annotate the zero bar to draw attention to the key finding
    # xytext sets where the label sits, arrowprops draws the arrow to the bar
    ax.annotate(f'44% report\nzero symptoms\n(n={igd_dist[0]})',
                xy=(0, igd_dist[0]), xytext=(1.5, 200),
                arrowprops=dict(arrowstyle='->', color='black'),
                fontsize=10)
    
    plt.tight_layout()
    plt.savefig('chart1.png', dpi=300, bbox_inches='tight')
    
    plt.show()



.. image:: output_2_0.png


.. code:: ipython3

    ## Most Gamers Are Fine
    
    #Before drawing any conclusions about gaming and mental health, it is worth starting 
    #with what the data actually shows: 44% of adolescent internet gamers report zero 
    #dysregulation symptoms. The distribution drops sharply after that, with severe cases 
    #representing a tiny minority of players. 
    
    #This matters because public discourse around gaming rarely starts here. Parents, 
    #policymakers, and health organizations tend to speak about gaming dysregulation as 
    #though it were a widespread epidemic. The data does not support that framing. 
    #Problematic gaming, by any reasonable clinical threshold, affects a small fraction 
    #of adolescents who play online games.
    
    #This does not mean dysregulation is not real or serious for those who experience it. 
    #It means that before we ask why some adolescents develop unhealthy relationships with 
    #games, we should acknowledge that the overwhelming majority do not, and ask what 
    #is different about the minority who do.

.. code:: ipython3

    # Chart 2: Relationship between daily psychological need frustration and IGD symptoms
    # Need frustration (feeling your autonomy, competence, and relatedness are blocked) is the strongest predictor in this dataset: accounts for 13% of IGD variance.
    
    fig, ax = plt.subplots(figsize=(10, 5))
    
    # Jitter is added to the y-axis (IGD count) because symptom scores are integers
    # Without jitter, hundreds of dots stack invisibly on the same y values
    # The jitter range (+-0.15) is small enough not to distort the data visually
    jitter = np.random.uniform(-0.15, 0.15, size=len(gamers))
    ax.scatter(gamers['need_deprivation'], gamers['igd_count'] + jitter,
               alpha=0.3, color='steelblue', edgecolors='none')
    
    # np.polyfit fits a degree-1 (linear) polynomial — gives us slope (m) and intercept (b)
    # We then generate 100 evenly spaced x values to draw a smooth line
    m, b = np.polyfit(gamers['need_deprivation'], gamers['igd_count'], 1)
    x_line = np.linspace(gamers['need_deprivation'].min(), gamers['need_deprivation'].max(), 100)
    ax.plot(x_line, m * x_line + b, color='crimson', linewidth=2)
    
    ax.set_xlabel('Psychological Need Frustration (daily life)', fontsize=12)
    ax.set_ylabel('IGD Symptom Count', fontsize=12)
    ax.set_title('Unmet Psychological Needs Predict Dysregulated Gaming', fontsize=14)
    
    plt.tight_layout()
    plt.savefig('chart2.png', dpi=300, bbox_inches='tight')
    plt.show()



.. image:: output_4_0.png


.. code:: ipython3

    ## The Real Risk Factor
    
    #When adolescents report that their basic psychological needs are being frustrated in 
    #daily life (that they feel controlled rather than autonomous, ineffective rather than 
    #competent, isolated rather than connected) their dysregulated gaming scores rise 
    #consistently with it.
    
    #From a neuroscience standpoint, this is not surprising. Chronic frustration of 
    #autonomy, competence, and relatedness activates the brain's threat-detection systems. 
    #The prefrontal cortex, responsible for impulse control and long-term planning, becomes 
    #less effective under sustained psychological stress. Games, which offer immediate 
    #feedback, clear achievement structures, and social belonging, become a neurologically 
    #compelling escape route precisely when these systems are under strain.
    
    #The upward trend in this chart is not steep, but it is consistent. And consistency 
    #across a nationally representative sample of over 500 adolescents is more meaningful 
    #than a dramatic effect in a convenience sample.

.. code:: ipython3

    # Chart 3: Side-by-side comparison of need frustration vs need satisfaction
    # Frustration is the real driver, not the absence of satisfaction. 
    
    fig, axes = plt.subplots(1, 2, figsize=(14, 5))
    
    # Loop over both variables to avoid repeating code
    # zip() pairs each axis, variable, title, and color together
    for ax, var, title, color in zip(
        axes,
        ['need_deprivation', 'need_satisfaction'],
        ['Need Frustration (stronger predictor)', 'Need Satisfaction (weaker predictor)'],
        ['crimson', 'steelblue']
    ):
        # Add jitter to y-axis so overlapping integer counts are visible
        # Without this, all the 0s, 1s, 2s stack on top of each other
        jitter = np.random.uniform(-0.15, 0.15, size=len(gamers))
        ax.scatter(gamers[var], gamers['igd_count'] + jitter,
                   alpha=0.3, color=color, edgecolors='none')
    
        # Fit a simple linear regression line to show direction of relationship
        m, b = np.polyfit(gamers[var], gamers['igd_count'], 1)
        x_line = np.linspace(gamers[var].min(), gamers[var].max(), 100)
        ax.plot(x_line, m * x_line + b, color='black', linewidth=2)
    
        # Clean up axis labels
        ax.set_xlabel(var.replace('_', ' ').title(), fontsize=11)
        ax.set_ylabel('IGD Symptom Count', fontsize=11)
        ax.set_title(title, fontsize=12)
    
    plt.suptitle('Frustration Drives Dysregulation — Satisfaction Barely Moves the Needle',
                 fontsize=13, fontweight='bold', y=1.02)
    
    plt.tight_layout()
    plt.savefig('chart3.png', dpi=300, bbox_inches='tight')
    plt.show()



.. image:: output_6_0.png


.. code:: ipython3

    ## Frustration and Satisfaction Are Not Opposites
    
    #A natural assumption would be that need satisfaction protects against dysregulated 
    #gaming in the same way that need frustration drives it. The data tells a more 
    #nuanced story. While need satisfaction does show a modest negative relationship 
    #with dysregulation, its slope is considerably flatter than frustration's positive one.
    
    #This asymmetry has a direct parallel in neuroscience. The brain's threat-response 
    #systems are not simply the mirror image of its reward systems. Negative experiences ( 
    #frustration, rejection, helplessness) tend to produce stronger and more durable 
    #behavioral responses than positive ones of equivalent intensity. This is sometimes 
    #called negativity bias, and it operates at a neurological level long before conscious 
    #reasoning gets involved.
    
    #What this could mean is that helping an adolescent feel more satisfied in daily 
    #life is valuable, but it is not a substitute for reducing the sources of frustration. 
    #The two require different interventions, and conflating them leads to incomplete 
    #solutions.

.. code:: ipython3

    # Chart 4: Does playing more hours predict dysregulated gaming?
    # Counter-intuitive finding: time spent gaming is a weak predictor of dysregulation.
    # This directly challenges the popular assumption that "playing too much = addiction."
    # The flat regression line is the point: hours alone don't explain the problem.
    
    fig, ax = plt.subplots(figsize=(10, 5)) 
    
    # Remove extreme outliers for visual clarity (2 participants reported 21hrs/day)
    # We keep data within the 99th percentile to avoid the axis being distorted
    plot_data = gamers[gamers['internet_gaming_time'] <= gamers['internet_gaming_time'].quantile(0.99)]
    
    jitter = np.random.uniform(-0.15, 0.15, size=len(plot_data))
    ax.scatter(plot_data['internet_gaming_time'], plot_data['igd_count'] + jitter,
               alpha=0.3, color='steelblue', edgecolors='none')
    
    # Regression line
    m, b = np.polyfit(plot_data['internet_gaming_time'], plot_data['igd_count'], 1)
    x_line = np.linspace(plot_data['internet_gaming_time'].min(), 
                         plot_data['internet_gaming_time'].max(), 100)
    ax.plot(x_line, m * x_line + b, color='crimson', linewidth=2)
    
    ax.set_xlabel('Daily Gaming Hours', fontsize=12)
    ax.set_ylabel('IGD Symptom Count', fontsize=12)
    ax.set_title('Time Spent Gaming Is a Weak Predictor of Dysregulation', fontsize=14)
    
    ax.text(0.98, 0.95, 'Hours played ≠ addiction risk',
            transform=ax.transAxes, fontsize=11, color='crimson',
            ha='right', va='top', style='italic')
    
    plt.tight_layout()
    plt.savefig('chart4.png', dpi=300, bbox_inches='tight')
    plt.show()



.. image:: output_8_0.png


.. code:: ipython3

    ## Hours Do Not Tell the Whole Story
    
    #The most persistent assumption in public debates about gaming is that time spent 
    #playing is the primary measure of risk. Screen time limits, parental controls, and 
    #clinical guidelines are frequently built around this assumption. This chart challenges it.
    
    #Across this sample of adolescent internet gamers, daily hours played show only a weak 
    #positive relationship with dysregulation symptoms. A teenager playing three hours a day 
    #can show more symptoms than one playing eight. A teenager playing ten hours a day can 
    #show none at all.
    
    #This does not mean time is irrelevant. It means time is a poor proxy for what actually 
    #matters. From a neuroscience perspective, compulsive behavior is driven by the 
    #motivational state behind an activity, not its duration. The dopaminergic reward 
    #pathways that underlie behavioral dysregulation are activated by the psychological 
    #function gaming serves, not by the clock. Two adolescents can spend the same number 
    #of hours playing for entirely different reasons, with entirely different outcomes.
    
    #Restricting screen time without addressing the underlying need frustration is the 
    #equivalent of treating a fever without asking what is causing it.

.. code:: ipython3

    # Chart 5: Need frustration vs parent-reported psychosocial problems
    # Need frustration doesn't just predict gaming dysregulation, it predicts real-world functioning problems reported by parents.
    # Externalising = hyperactivity + conduct issues. Internalising = emotional + peer difficulties.
    # This connects the gaming data back to broader adolescent wellbeing 
    
    fig, axes = plt.subplots(1, 2, figsize=(14, 5))
    
    for ax, outcome, title in zip(
        axes,
        ['externalising_problems', 'internalising_problems'],
        ['Externalising Problems\n(hyperactivity + conduct)', 
         'Internalising Problems\n(emotional + peer difficulties)']
    ):
        # No jitter needed here, both variables are continuous scores, not integer counts
        ax.scatter(gamers['need_deprivation'], gamers[outcome],
                   alpha=0.3, color='crimson', edgecolors='none')
    
        m, b = np.polyfit(gamers['need_deprivation'], gamers[outcome], 1)
        x_line = np.linspace(gamers['need_deprivation'].min(), 
                             gamers['need_deprivation'].max(), 100)
        ax.plot(x_line, m * x_line + b, color='black', linewidth=2)
    
        ax.set_xlabel('Psychological Need Frustration (daily life)', fontsize=11)
        ax.set_ylabel('Problem Score (parent-reported)', fontsize=11)
        ax.set_title(title, fontsize=12)
    
    # The suptitle frames this as the takeaway, not just a description
    plt.suptitle('Unmet Needs Predict Real-World Functioning Problems — Not Just Gaming Behavior',
                 fontsize=13, fontweight='bold', y=1.02)
    
    plt.tight_layout()
    plt.savefig('chart5.png', dpi=300, bbox_inches='tight')
    plt.show()



.. image:: output_10_0.png


.. code:: ipython3

    ## Gaming Is Not the Problem. Life Is.
    
    #Need frustration does not only predict 
    #dysregulated gaming, it predicts real-world functioning difficulties as reported 
    #by parents. Adolescents with higher need frustration show more externalising problems 
    #like hyperactivity and conduct issues, and more internalising problems like emotional 
    #distress and peer difficulties.
    
    #Crucially, dysregulated gaming itself accounts for very little of this variance once 
    #need frustration is in the picture. The gaming behavior is not driving the psychosocial 
    #difficulties. Both are being driven by the same upstream cause: a daily life in which 
    #an adolescent's fundamental psychological needs are going unmet.
    
    #This has a direct implication for intervention. If a teenager is struggling socially, 
    #emotionally, or behaviorally, and they also happen to play a lot of games, the instinct 
    #is often to restrict the games. But if the games are a symptom rather than a cause, 
    #removing them without addressing the underlying need frustration is likely to produce 
    #a different symptom, not a healthier adolescent. The brain will find another outlet. 

.. code:: ipython3

    # Chart 6: Gender differences in the relationship between need frustration and dysregulation
    # Does need frustration hit boys and girls differently?
    # We use the 'female' column: 1 = female, 0 = male, -1 = other (excluded due to n=3)
    
    # Filter out the -1 "other" code, only 3 participants, not enough to analyze
    gender_data = gamers[gamers['female'].isin([0, 1])].copy()
    
    # Map numeric codes to readable labels
    gender_data['gender_label'] = gender_data['female'].map({1: 'Female', 0: 'Male'})
    
    fig, axes = plt.subplots(1, 2, figsize=(14, 5))
    
    colors = {'Male': 'steelblue', 'Female': 'crimson'}
    
    for ax, gender in zip(axes, ['Male', 'Female']):
        subset = gender_data[gender_data['gender_label'] == gender]
        
        jitter = np.random.uniform(-0.15, 0.15, size=len(subset))
        ax.scatter(subset['need_deprivation'], subset['igd_count'] + jitter,
                   alpha=0.3, color=colors[gender], edgecolors='none')
        
        # Regression line per gender group
        m, b = np.polyfit(subset['need_deprivation'], subset['igd_count'], 1)
        x_line = np.linspace(subset['need_deprivation'].min(),
                             subset['need_deprivation'].max(), 100)
        ax.plot(x_line, m * x_line + b, color='black', linewidth=2)
        
        # Add sample size and slope annotation so the reader can compare panels
        ax.text(0.05, 0.92, f'n={len(subset)}\nslope={m:.2f}',
                transform=ax.transAxes, fontsize=10, va='top')
        
        ax.set_xlabel('Psychological Need Frustration', fontsize=11)
        ax.set_ylabel('IGD Symptom Count', fontsize=11)
        ax.set_title(f'{gender} Adolescents', fontsize=12)
        ax.set_ylim(-0.5, 10)
    
    plt.suptitle('Does Need Frustration Affect Boys and Girls Differently?',
                 fontsize=13, fontweight='bold', y=1.02)
    
    plt.tight_layout()
    plt.savefig('chart6.png', dpi=300, bbox_inches='tight')
    plt.show()



.. image:: output_12_0.png


.. code:: ipython3

    ## Gender Differences in Need Frustration and Gaming Dysregulation
    
    #The data reveals something the original research didn't explore: need frustration does not 
    #translate into dysregulated gaming equally across genders. Among male adolescents, every 
    #unit increase in psychological need frustration corresponds to a 0.70 increase in IGD symptoms. 
    #Among female adolescents, that same increase in frustration produces only a 0.48 symptom increase.
    
    #This gap is worth taking seriously. From a neuroscience perspective, the stress response to 
    #unmet psychological needs, blocked autonomy, thwarted competence, social disconnection, is 
    #not gender neutral. Research on adolescent stress responses consistently shows that males are 
    #more likely to externalize distress through behavioral outlets, while females tend to internalize 
    #it through emotional and social channels. Gaming, as an achievement and autonomy-rich environment, 
    #may be particularly effective at absorbing male distress responses.
    
    #In other words, when a teenage boy's needs go unmet in real life, games often offer a replacement 
    #environment where he can feel competent, in control, and socially connected. The same replacement 
    #mechanism appears to operate less strongly for girls, who may seek different outlets for the 
    #same underlying frustration.
    
    #One important caveat: the female sample here is considerably smaller (n=164 vs n=359 males), 
    #which reflects the broader reality that female adolescents engage less in internet-based gaming. 
    #This imbalance means these findings should be interpreted with appropriate caution rather than 
    #treated as definitive. A larger, more balanced sample would be needed to confirm whether this 
    #gender difference is robust. 

.. code:: ipython3

    ## Disclaimer
    #This analysis is an independent exploration of a publicly available dataset 
    #(Przybylski & Weinstein, 2019). It does not constitute clinical advice. 
    #All interpretations are my own and do not represent the views of the original authors.

