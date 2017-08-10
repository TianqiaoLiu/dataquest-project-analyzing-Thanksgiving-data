# dataquest-project-analyzing-Thanksgiving-data
1. Introducing Thanksgiving Dinner Data 2. Filtering Out Rows From A DataFrame 3. Using value_counts To Explore Main Dishes 4. Figuring Out What Pies People Eat 5. Converting Age To Numeric 6. Converting Income To Numeric 7. Correlating Travel Distance And Income 8. Linking Friendship And Age

import pandas
data = pandas.read_csv('thanksgiving.csv', encoding = 'Latin-1')
print(data[0:5])

print(data.columns)

data['What is typically the main dish at your Thanksgiving dinner?'].value_counts()
data[data['What is typically the main dish at your Thanksgiving dinner?'] == 'Tofurkey']['Do you typically have gravy?']
apple_isnull = data['Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Apple'].isnull()
print(apple_isnull)
pumpkin_isnull = data['Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Pumpkin'].isnull()
pecan_isnull = data['Which type of pie is typically served at your Thanksgiving dinner? Please select all that apply. - Pecan'].isnull()
ate_pies = apple_isnull & pumpkin_isnull & pecan_isnull
print(ate_pies.unique())
occur = dict()
ate_pies.value_counts()


def s_2_i(str_1):
    if pandas.isnull(str_1):
        return None
    else:
        str_2 = str_1.split()[0]
        len_2 = len(str_2)
        if str_2[len_2 - 1] == '+':
            str_2 = str_2[:len_2-1]
    return int(str_2)
data['int_age'] = data['Age'].apply(s_2_i)
data['int_age'].describe()


def string_2_int(str_1):
    if pandas.isnull(str_1):
        return None
    str_2 = str_1.split(' ')[0]
    if str_2 == 'Prefer':
        return None
    else:
        str_2 = str_2.replace('$','')
        str_2 = str_2.replace(',','')
    return int(str_2)
data['int_income'] = data['How much total combined money did all members of your HOUSEHOLD earn last year?'].apply(string_2_int)
print(data['int_income'])
data['int_income'].describe

x = data['int_income'] < 150000
data[x]['How far will you travel for Thanksgiving?'].value_counts()

x = data['int_income'] > 150000
data[x]['How far will you travel for Thanksgiving?'].value_counts()

data.pivot_table(index = 'Have you ever tried to meet up with hometown friends on Thanksgiving night?',columns = 'Have you ever attended a "Friendsgiving?"', values = 'int_age')

data.pivot_table(index='Have you ever tried to meet up with hometown friends on Thanksgiving night?', columns='Have you ever attended a "Friendsgiving?"', values='int_income')
