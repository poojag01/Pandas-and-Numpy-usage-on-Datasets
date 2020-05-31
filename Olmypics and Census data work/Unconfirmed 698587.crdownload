
# The following code loads the olympics dataset (olympics.csv), which was derrived from the Wikipedia entry on
# [All Time Olympic Games Medals](https://en.wikipedia.org/wiki/All-time_Olympic_Games_medal_table), and does some basic data cleaning. 
# 
# The columns are organized as # of Summer games, Summer medals, # of Winter games, Winter medals, total # number of games,
# total # of medals. 

import pandas as pd

df = pd.read_csv('olympics.csv', index_col=0, skiprows=1)

for col in df.columns:
    if col[:2]=='01':
        df.rename(columns={col:'Gold'+col[4:]}, inplace=True)
    if col[:2]=='02':
        df.rename(columns={col:'Silver'+col[4:]}, inplace=True)
    if col[:2]=='03':
        df.rename(columns={col:'Bronze'+col[4:]}, inplace=True)
    if col[:1]=='№':
        df.rename(columns={col:'#'+col[1:]}, inplace=True)

names_ids = df.index.str.split('\s\(') # split the index by '('

df.index = names_ids.str[0] # the [0] element is the country name (new index) 
df['ID'] = names_ids.str[1].str[:3] # the [1] element is the abbreviation or ID (take first 3 characters from that)

df = df.drop('Totals')
df

# ### Question 0

# What is the first country in df?

def answer_zero():
    # This function returns the row for Afghanistan, which is a Series object.
    return df.iloc[0]
answer_zero() 


# ### Question 1

# Which country has won the most gold medals in summer games?
# 
# *This function should return a single string value.*


def answer_one():    
    return df.Gold.idxmax()
answer_one()



# ### Question 2

# Which country had the biggest difference between their summer and winter gold medal counts?
# 
# *This function should return a single string value.*

def answer_two():
    return ( df['Gold'] -  df['Gold.1']).idxmax()
    #return (df.Gold - df.(Gold.1) ).idxmax()   #"YOUR ANSWER HERE"
#answer_two()



# ### Question 3

# Which country has the biggest difference between their summer gold medal counts and winter gold medal counts relative to their total gold medal count? 
# 
# $$\frac{Summer~Gold - Winter~Gold}{Total~Gold}$$
# 
# Only include countries that have won at least 1 gold in both summer and winter.
# 
# *This function should return a single string value.*

def answer_three():
    temp = df[ df.Gold > 0]
    temp = temp[ temp['Gold.1'] > 0]
    return ( ( temp['Gold'] - temp['Gold.1'])/ ( temp['Gold'] + temp['Gold.1'])).idxmax()
    #temp = temp[ temp.Gold.1 > 0]
    #return ( ( temp.Gold - temp.Gold.1)/ ( temp.Gold + temp.Gold.1)).idxmax()   #"YOUR ANSWER HERE"

answer_three()
#df.columns


# ### Question 4

# Write a function that creates a Series called "Points" which is a weighted value where each gold medal (`Gold.2`) counts for 3 points, silver medals (`Silver.2`) for 2 points, and bronze medals (`Bronze.2`) for 1 point. The function should return only the column (a Series object) which you created, with the country names as indices.
# 
# *This function should return a Series named `Points` of length 146*


def answer_four():
#    for i in df :
#        print (i)
        
    Points = df['Gold.2'] *3 + df['Silver.2']*2 + df['Bronze.2'] 
    #print (type(Points) )
    return Points 
answer_four()


# ## Part 2
# For the next set of questions, we will be using census data from the [United States Census Bureau](http://www.census.gov).
# Counties are political and geographic subdivisions of states in the United States.
# This dataset contains population data for counties and states in the US from 2010 to 2015.
# [ See this document](https://www2.census.gov/programs-surveys/popest/technical-documentation/file-layouts/2010-2015/co-est2015-alldata.pdf) for a description of the variable names.
# 


census_df = pd.read_csv('census.csv')
census_df
#census_df.columns
#census_df.describe



# ### Question 5

# Which state has the most counties in it? 
# 
# *This function should return a single string value.*

def answer_five() :
    temp = census_df[census_df.SUMLEV == 50 ]
    temp = temp.groupby("STNAME").count()["CTYNAME"]
    return temp.idxmax()



# ### Question 6

# **Only looking at the three most populous counties for each state**, what are the three most populous states (in order of highest population to lowest population)? Use `CENSUS2010POP`.
# 
# *This function should return a list of string values.*


#def answer_six() :
#    temp = census_df[ census_df['SUMLEV'] == 50 ]
#    #temp = census_df.sort_values( by = 'CENSUS2010POP' , ascending = False)
#    temp = temp.sort_values( by = 'CENSUS2010POP' , ascending = False)
#    temp = temp.groupby('STNAME')
#    #list( temp)
#    #list.reverse(temp)
#    return list (temp.head(3)['STNAME'] )[ : 3]
#answer_six()
def answer_six() :
    temp = census_df.groupby('STNAME')['CENSUS2010POP'].nlargest(3).sum(level = 0)
    
    tt = []
    for x in temp[0 : 3].keys() :
        tt.append( x)
#    print(type(tt) )
#    return tt
    #print (set(temp.keys().unique()[:3] ) )
    return temp[0 : 3].keys().tolist()    #working correct but returning object which is not required here
answer_six()



# ### Question 7

# Which county has had the largest absolute change in population within the period 2010-2015? (Hint: population values are stored in columns POPESTIMATE2010 through POPESTIMATE2015, you need to consider all six columns.)
# 
# e.g. If County Population in the 5 year period is 100, 120, 80, 105, 100, 130, then its largest change in the period would be |130-80| = 50.
# 
# *This function should return a single string value.*

def answer_seven() :
    temp = census_df[ census_df['SUMLEV'] == 50]
    temp = temp.set_index(['CTYNAME'])
#    print(temp.ndim )
    column = ['POPESTIMATE2010', 'POPESTIMATE2011', 'POPESTIMATE2012', 'POPESTIMATE2013', 'POPESTIMATE2014', 'POPESTIMATE2015' ]
    temp_max = temp[column].max(axis = 1)
    temp_min = temp[column].min(axis = 1)
    temp_diff = temp_max - temp_min
#    print( type(temp_diff))
#    print( temp_diff.idxmax() )

    return ( temp_diff.idxmax() )
    

# ### Question 8
# In this datafile, the United States is broken up into four regions using the "REGION" column. 
# 
# Create a query that finds the counties that belong to regions 1 or 2, whose name starts with 'Washington', and whose POPESTIMATE2015 was greater than their POPESTIMATE 2014.
# 
# *This function should return a 5x2 DataFrame with the columns = ['STNAME', 'CTYNAME'] and the same index ID as the census_df (sorted ascending by index).*

def answer_eight():
    filter1 = (census_df['CTYNAME'].str.startswith('Washington') )
    filter2 = ( census_df['POPESTIMATE2015'] > census_df['POPESTIMATE2014'] )
    sel_col = ['STNAME' , 'CTYNAME'] 
    temp = census_df [ filter1 & filter2 ]
    temp
    reg1 =  temp['REGION'] == 1 
    reg2 =  temp['REGION'] == 2 
    reg = temp[ reg1 | reg2 ][ sel_col]
#    print( reg1.head(5))
#    temp = temp.set_index(['STNAME'])
#    print ( temp['STNAME'].unique())
#    print( temp)
    return reg
