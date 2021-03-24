# Research

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

urlfile="https://raw.githubusercontent.com/ghamut/automated-venture-capitalist/master/data/individualData.csv"

### Step1: Reading the data and displaying part of it

df = pd.read_csv(urlfile)

df.head()

ft = pd.DataFrame(data=None)


ft['team_id']=df['team_id']


### Step 2 Computing the features of the data

# #Create array to store featues 
# print(df.team_id)

# s=[];



#% INDIVIDUAL-LEVEL FEATURES: 8 in total %
#Harvard or MIT?
# df2['date'] = df1['date'].values
ft['is_mit']=df['is_mit']

ft['is_harvard']=df['is_harvard']

#How long since graduation?

ft['years_since_graduation']=df['years_since_graduation']


#Do you Hold an MBA and/or a PhD?

ft['is_mba']=df['is_mba']*1

ft['is_phd']=df['is_phd']

###Q: Why do we multiply by 1 for mba

#Did you study Math Science or Engineering?

ft['is_stem']=((df['major_engineer'] == 1) | (df['major_scientist'] == 1) | (df['major_mathecon'] == 1)).replace(True, 1)
# s['is_stem'] = 1* d[['major_engineer','major_scientist','major_mathecon']]

ft

#How many of the the 7 soft skills do you have?

ft['skills_soft']= (df.skill_class_creative + df.skill_class_design + df.skill_class_communication +  df.skill_class_legal +df.skill_class_management + df.skill_class_relationship + df.skill_class_education)/7
 

#Do people consistently think you look competent?

###NOTE: REMEMBER ITS BOOLEAN###

ft["pic_rating_total_3 "]= df.pic_rating_total == 3

ft

##% TEAM LEVEL FEATURES: 8 Total  %%##

##% Do at least 40% of people prefer your idea and team name to random other ideas?##
ft["votes_threshold_2min"] = df.votes_threshold_2min/10;


#% Was the idea something that would be used everyday, periodically, or rarely. 
ft["topic_inessential"] = df.topic_food | df.topic_goods | df.topic_impact | df.topic_entertainment 
ft["topic_essentialperiodic"]  =  df.topic_employment | df.topic_information | df.topic_children | df.topic_social |  df.topic_health | df.topic_transportation 
ft["topic_essentialdaily"]  = df.topic_finance | df.topic_energy 

#% How much did the the author directly engage with the reader?#
df["POS_period"].replace(0,1,inplace=True)
                    
ft["wd_speaking_to_you"]= df.wd_you/ df.POS_period

ft["wd_questions"]= df.wd_questions/df.POS_period


#% Were they descriptive?
ft["wd_adjectives"] = df.wd_adjective/df.POS_period

#% Were they positive?
ft['positive_sentiment'] = df.sent_Verypositive/df.POS_period + df.sent_Positive/df.POS_period

#% OUTCOMES: nominated, finalist, and who was successful %#
ft["nomination"] = df.nomination;
ft["success"] = df.success
ft["finalist"] = df.finalist



##d=s;### what is this expression here and what does it mean? So ft is the new df


### STEP 3 Condensing features into team level###

ft3 = ft.groupby('team_id').mean()

ft3



# creating new DF t
# t= pd.DataFrame(data=None)
# #pulling out unique an putting them in array calles team ids
# team_ids = ft.team_id.unique()



# #iterating over teams
# for i in range(len(team_ids)):
    
#     #pulling teams 1 by 1
#     this_team_id = team_ids[i]
#     #pulling out rows(lists) of people that belong to the team#
    
#     these_members = ft(ft.team_id == this_team_id).values
#     these_members = these_members[these_members==True]  
#     print(these_members)
    




