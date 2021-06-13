# ODI_Data_Visualization
import numpy as np # linear algebra
import pandas as pd # data processing
import matplotlib.pyplot as plt
import seaborn as sns
import networkx as nx 
Continuous=pd.read_csv('C:/Users/SUBHROJIT_PC/Desktop/96-DSP212-190022-Information Visualisation Laboratory/ContinousDataset.csv')
Continuous.head(5)
Continuous.loc[(Continuous["Match Date"] == "Jan 5, 1971"),:] 
Continuous.head(5)
def hwin(s):
      if (s['Winner'] == s['Team 1']  and s['Venue_Team1'] == "Home"):
              return s['Team 1']
      elif (s['Winner'] == s['Team 2']  and s['Venue_Team2'] == "Home"):
              return s['Team 2']              
      else :
          return np.nan

def awin(s):
      if (s['Winner'] == s['Team 1']  and s['Venue_Team1'] in( "Away",'Neutral')):
              return s['Team 1']
      elif (s['Winner'] == s['Team 2']  and s['Venue_Team2'] in( "Away",'Neutral')):
              return s['Team 2']
              
      else :
          return np.nan

def hloss(s):
      if (s['Winner'] != s['Team 1']  and s['Venue_Team1'] == "Home"):
              return s['Team 1']
      elif (s['Winner'] != s['Team 2']  and s['Venue_Team2'] == "Home"):
              return s['Team 2']
              
      else :
          return np.nan

def aloss(s):
      if (s['Winner'] != s['Team 1']  and s['Venue_Team1'] in( "Away",'Neutral')):
              return s['Team 1']
      elif (s['Winner'] != s['Team 2']  and s['Venue_Team2'] in( "Away",'Neutral')):
              return s['Team 2']          
      else :
          return np.nan
        
def FirstInningsWinner(s):
      if (s['Winner'] == s['Team 1']  and s['Innings_Team1'] in( "First")):
              return s['Team 1']
      elif (s['Winner'] == s['Team 2']  and s['Innings_Team2'] in( "First")):
              return s['Team 2']          
      else :
          return np.nan
Continuous['Year']=Continuous['Match Date'].str[-4:]
matches_played_byteams=pd.concat([Continuous['Team 1'],Continuous['Team 2']])
matches_played_byteams=matches_played_byteams.value_counts().reset_index()
matches_played_byteams.columns=['Team','Total Matches']
Continuous['HomeWin']= Continuous.apply(hwin,axis=1)
Continuous['AwayWin']= Continuous.apply(awin,axis=1)
Continuous['HomeLoss']= Continuous.apply(hloss,axis=1)
Continuous['AwayLoss']= Continuous.apply(aloss,axis=1)
Continuous['FistInningsWinner']= Continuous.apply(FirstInningsWinner,axis=1)
matches_played_byteams.set_index('Team',inplace=True)
Winner=Continuous['Winner'].value_counts().sort_values( ascending= False ).reset_index()
Winner.set_index('index',inplace=True)
HomeWin=Continuous['HomeWin'].value_counts().sort_values( ascending= False ).reset_index()
HomeWin.set_index('index',inplace=True)
AwayWin=Continuous['AwayWin'].value_counts().sort_values( ascending= False ).reset_index()
AwayWin.set_index('index',inplace=True)
HomeLoss=Continuous['HomeLoss'].value_counts().sort_values( ascending= False ).reset_index()
HomeLoss.set_index('index',inplace=True)
AwayLoss=Continuous['AwayLoss'].value_counts().sort_values( ascending= False ).reset_index()
AwayLoss.set_index('index',inplace=True)
FistInningsWinner=Continuous['FistInningsWinner'].value_counts().sort_values( ascending= False ).reset_index()
FistInningsWinner.set_index('index',inplace=True)
matches_played_byteams['wins']=Winner['Winner']/2
matches_played_byteams['HomeWin']=HomeWin['HomeWin']/2
matches_played_byteams['AwayWin']=AwayWin['AwayWin']/2
matches_played_byteams['HomeLoss']=HomeLoss['HomeLoss']/2
matches_played_byteams['AwayLoss']=AwayLoss['AwayLoss']/2
matches_played_byteams['FistInningsWinner']=FistInningsWinner['FistInningsWinner']/2
matches_played_byteams['SecondInningsWinner']=matches_played_byteams['wins']-matches_played_byteams['FistInningsWinner']
matches_played_byteams['HomeTotal']=matches_played_byteams['HomeWin']+ matches_played_byteams['HomeLoss']
matches_played_byteams['AwayTotal']=matches_played_byteams['AwayWin']+ matches_played_byteams['AwayLoss']
print(matches_played_byteams)
barWidth = 0.85
plt.subplots(figsize=(12,6))
plt.bar(matches_played_byteams.index, matches_played_byteams['Total Matches'], color='blue', edgecolor='white', width=barWidth)
plt.bar(matches_played_byteams.index, matches_played_byteams['wins'], color='yellow', edgecolor='red', width=barWidth)
plt.xticks(matches_played_byteams.index, matches_played_byteams.index,rotation='vertical')
plt.xlabel("group")
plt.title('Total Match vs wins',size=25)
plt.show()  
barWidth = 0.85
plt.subplots(figsize=(12,6))
plt.bar(matches_played_byteams.index, matches_played_byteams['HomeTotal'], color='red', edgecolor='white', width=barWidth)
plt.bar(matches_played_byteams.index, matches_played_byteams['HomeWin'], color='yellow', edgecolor='black', width=barWidth)
plt.xticks(matches_played_byteams.index, matches_played_byteams.index,rotation='vertical')
plt.xlabel("group")
plt.title('Home Ground vs wins',size=25)
plt.show() 
s=matches_played_byteams[['FistInningsWinner','SecondInningsWinner']].plot(kind="bar",figsize=(12,6),fontsize=12) 
plt.xlabel("group")
plt.title('InningWisewins',size=25)
plt.legend(loc = 'upper right')
plt.show()
barWidth = 0.85
plt.subplots(figsize=(12,6))
plt.bar(matches_played_byteams.index, matches_played_byteams['AwayTotal'], color='blue', edgecolor='white', width=barWidth)
plt.bar(matches_played_byteams.index, matches_played_byteams['AwayWin'], color='orange', edgecolor='red', width=barWidth)
plt.xticks(matches_played_byteams.index, matches_played_byteams.index,rotation='vertical')
plt.xlabel("group")
plt.title('Outside Ground vs Wins',size=25)
plt.show() 
DFGrounds=Continuous['Ground'].value_counts().reset_index()
DFGrounds.columns=['Ground','Total Matches']
DFGrounds = DFGrounds.sort_values(by ='Total Matches', ascending = False)[:20]
DFGrounds.set_index('Ground',inplace=True)
barWidth = 0.85
plt.subplots(figsize=(12,6))
plt.bar(DFGrounds.index, DFGrounds['Total Matches'], color='indigo', edgecolor='white', width=barWidth)
plt.xticks(rotation='vertical')
plt.xlabel("group")
plt.title('Total Matches played per ground',size=25)
plt.show()
HostCountry=Continuous['Host_Country'].value_counts().reset_index()
HostCountry.columns=['Host_Country','Total Matches']
HostCountry = HostCountry.sort_values(by ='Total Matches', ascending = False)
HostCountry.set_index('Host_Country',inplace=True)
barWidth = 0.85
plt.subplots(figsize=(12,6))
plt.bar(HostCountry.index, HostCountry['Total Matches'], color='red', edgecolor='white', width=barWidth)
plt.xticks(rotation='vertical')
plt.xlabel("group")
plt.title('Total Matches hosted by Country',size=25)
plt.show()
Year=Continuous['Year'].value_counts().sort_values( ascending= False ).reset_index()
Year.columns=['Year','Total Matches']
Year.set_index('Year',inplace=True)
print(Year)
barWidth = 0.85
plt.subplots(figsize=(12,6))
plt.bar(Year.index, Year['Total Matches'], color='blue', edgecolor='white', width=barWidth)
plt.xticks(Year.index, Year.index,rotation='vertical')
plt.xlabel("group")
plt.title('Total Matches per year',size=25)
plt.show() 
k = np.array([475,1285])
mylables = ["Wins","Loss or Tie"]

plt.pie(k, labels= mylables)
plt.show()
