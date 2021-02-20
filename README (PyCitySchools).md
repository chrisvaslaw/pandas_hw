# pandas_hw
### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.

# Dependencies and Setup
import pandas as pd
import numpy as np

# File to Load (Remember to Change These)
school_data_to_load = "Resources/schools_complete.csv"
student_data_to_load = "Resources/students_complete.csv"

# Read School and Student Data File and store into Pandas DataFrames
school_data = pd.read_csv(school_data_to_load)
student_data = pd.read_csv(student_data_to_load)

# Combine the data into a single dataset.  
school_data_complete = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])

school_data_complete.count()

school_data_complete.head(10)

# District Summary

# Calculate the total number of schools

total_schools = len(school_data_complete["school_name"].unique())
total_schools

# Calculate the total number of students

total_students = school_data_complete["Student ID"].count()
total_students

# Calculate the total budget

total_budget = school_data["budget"].sum()
total_budget

# Calculate the average math score

average_math = school_data_complete["math_score"].mean()
average_math

# Calculate the average reading score

average_reading = school_data_complete["reading_score"].mean()
average_reading

# Calculate the percentage of students with a passing math score (70 or greater)

good_math_students = school_data_complete.loc[school_data_complete["math_score"] > 69, ["student_name", "math_score"]]
good_math_score = good_math_students["math_score"].count()
good_math_percentage = (good_math_score / total_students) * 100

# Calculate the percentage of students with a passing reading score (70 or greater)

good_reading_students = school_data_complete.loc[school_data_complete["reading_score"] > 69, ["student_name", "reading_score"]]  
good_reading_score = good_reading_students["reading_score"].count()
good_reading_percentage = (good_reading_score / total_students) *100

# Calculate the percentage of students who passed math and reading (% Overall Passing)

school_data_complete.loc[:, ["Student ID", "reading_score", "math_score"]].head()  
only_good_students = school_data_complete.loc[(school_data_complete["reading_score"] > 69) & (school_data_complete["math_score"] > 69), :]
only_good_students_count = only_good_students["Student ID"].count()
good_student_percentage = (only_good_students_count / total_students) * 100

# Create a dataframe to hold the above results

student_data_summary_df = pd.DataFrame({"Total Schools": [total_schools], "Total Students": 
                                [total_students], "Total Budget": [total_budget], 
                                "Avg. Math Score": [average_math], "Avg. Reading Score": [average_reading],
                                "% Passing Math": [good_math_percentage], "% Passing Reading": [good_reading_percentage],
                                 "% Passing Both": [good_student_percentage]})
student_data_summary_df

# Optional: give the displayed data cleaner formatting

student_data_summary_df.style.format({"Total Budget":"{:,.2f}",
                                  "Avg. Math Score":"{:,.2f}",
                                  "Avg. Reading Score":"{:,.2f}",
                                  "% Passing Math":"{:,.2f}",
                                  "% Passing Reading":"{:,.2f}",
                                  "% Passing Both":"{:,.2f}"
                                   })

# School Summary

# School Name

total_schools = school_data_complete["school_name"].unique()
total_schools

# School Type

school_type = school_data_complete["type"].unique()
school_type

# Group by Schools

grouped_by_schools = school_data_complete.groupby("school_name")
grouped_by_schools.max()

# Total Students

student_count = grouped_by_schools["Student ID"].count()
student_count

# Total School Budget

total_school_budget = grouped_by_schools["budget"].unique()
total_school_budget

# Per Student Budget

per_student_budget = total_school_budget / student_count
per_student_budget

# Average Math Score

average_math_schools = grouped_by_schools["math_score"].mean()
average_math_schools

# Average Reading Score

average_reading_schools = grouped_by_schools["reading_score"].mean()
average_reading_schools

# Passing Math

good_math_school_percentage = school_data_complete[school_data_complete['math_score'] >= 70].groupby('school_name')['Student ID'].count()/student_count
good_math_school_percentage_formatted = good_math_school_percentage * 100

# Passing Reading

good_reading_school_percentage = school_data_complete[school_data_complete['reading_score'] >= 70].groupby('school_name')['Student ID'].count()/student_count
good_reading_school_percentage_formatted = good_reading_school_percentage * 100

# Overall Passing (The percentage of students that passed math and reading.)

good_overall_school_percentage = school_data_complete[(school_data_complete['reading_score'] >= 70) & (school_data_complete['math_score'] >= 70)].groupby('school_name')['Student ID'].count()/student_count
good_overall_school_percentage_formatted = good_overall_school_percentage * 100
good_overall_school_percentage_formatted

# Create a dataframe to hold the above results

school_data_summary_df = pd.DataFrame({
                                "Total Students": student_count,
                                "Total Budget": [list[0] for list in total_school_budget], 
                                "Budget Per Student": [list[0] for list in per_student_budget], 
                                "Avg. Math Score": average_math_schools, 
                                "Avg. Reading Score": average_reading_schools,
                                "% Passing Math": good_math_school_percentage_formatted,
                                "% Passing Reading": good_reading_school_percentage_formatted,
                                "% Passing Overall": good_overall_school_percentage_formatted})
school_data_summary_df

# Sort and display the top five performing schools by % overall passing.

school_data_summary_df_descending = school_data_summary_df.sort_values("% Passing Overall", ascending=False)
school_data_summary_df_descending.head()

# Sort and display the five worst-performing schools by % overall passing.

school_data_summary_df_ascending = school_data_summary_df.sort_values("% Passing Overall", ascending=True)
school_data_summary_df_ascending.head()

#  Math Scores by Grade
# Create a table that lists the average Math Score for students of each grade level (9th, 10th, 11th, 12th) at each school.

# Create a pandas series for each grade. Hint: use a conditional statement.

math_ninth = school_data_complete.loc[school_data_complete["grade"] == '9th'].groupby("school_name")["math_score"].mean()
math_tenth = school_data_complete.loc[school_data_complete["grade"] == '10th'].groupby("school_name")["math_score"].mean()
math_eleventh = school_data_complete.loc[school_data_complete["grade"] == '11th'].groupby("school_name")["math_score"].mean()
math_twelfth = school_data_complete.loc[school_data_complete["grade"] == '12th'].groupby("school_name")["math_score"].mean()

# Group each series by school


math_grade_summary_df = pd.DataFrame({
                                "Ninth Grade": math_ninth,
                                "Tenth Grade": math_tenth, 
                                "Eleventh Grade": math_eleventh, 
                                "Twelfth Grade": math_twelfth
                                    })
math_grade_summary_df.style.format({"Ninth Grade":"{:,.2f}",
                                   "Tenth Grade":"{:,.2f}",
                                   "Eleventh Grade":"{:,.2f}",
                                   "Twelfth Grade":"{:,.2f}",
                                   })

# Perform the same operations as above for reading scores

reading_ninth = school_data_complete.loc[school_data_complete["grade"] == '9th'].groupby("school_name")["reading_score"].mean()
reading_tenth = school_data_complete.loc[school_data_complete["grade"] == '10th'].groupby("school_name")["reading_score"].mean()
reading_eleventh = school_data_complete.loc[school_data_complete["grade"] == '11th'].groupby("school_name")["reading_score"].mean()
reading_twelfth = school_data_complete.loc[school_data_complete["grade"] == '12th'].groupby("school_name")["reading_score"].mean()

reading_grade_summary_df = pd.DataFrame({
                                "Ninth Grade": reading_ninth,
                                "Tenth Grade": reading_tenth, 
                                "Eleventh Grade": reading_eleventh, 
                                "Twelfth Grade": reading_twelfth
                                    })
reading_grade_summary_df.style.format({"Ninth Grade":"{:,.2f}",
                                   "Tenth Grade":"{:,.2f}",
                                   "Eleventh Grade":"{:,.2f}",
                                   "Twelfth Grade":"{:,.2f}",
                                   })

# Create a table that breaks down school performances based on average Spending Ranges (Per Student). Use 4 reasonable bins to group school spending. Include in the table each of the following:

print(school_data_summary_df ["Budget Per Student"].max())
print(school_data_summary_df ["Budget Per Student"].min())

bins = [0, 599, 649, 699]
group_names = ["550 to 599", "600 to 649", "650 to 699"]

school_data_complete["Spend Summary"] = pd.cut(school_data_complete["budget"]/school_data_complete["size"], bins, labels=group_names, include_lowest=True)
group_by_spend = school_data_complete.groupby("Spend Summary")

# Average Math Score

average_math_ = group_by_spend["math_score"].mean()

# Average Reading Score

average_reading_ = group_by_spend["reading_score"].mean()

# % Passing Math

passing_math = school_data_complete.loc[school_data_complete["math_score"] >= 70].groupby("Spend Summary")["Student ID"].count()/group_by_spend['Student ID'].count()
passing_math_formatted = passing_math  * 100

# % Passing Reading

passing_reading = school_data_complete.loc[school_data_complete["reading_score"] >= 70].groupby("Spend Summary")["Student ID"].count()/group_by_spend['Student ID'].count()
passing_reading_formatted = passing_reading * 100

# Overall Passing Rate (Average of the above two)

overall = school_data_complete[(school_data_complete['reading_score'] >= 70) & (school_data_complete['math_score'] >= 70)].groupby('Spend Summary')['Student ID'].count()/group_by_spend['Student ID'].count()
overall_formatted = overall * 100

spend_summary_df = pd.DataFrame({
                                   "Average Math Score": average_math_ ,
                                   "Average Reading Score": average_reading_,
                                   '% Passing Math': passing_math_formatted,
                                   '% Passing Reading': passing_reading_formatted,
                                   'Overall Passing %': overall_formatted
                                   })

spend_summary_df.style.format({"Average Math Score":"{:,.2f}",
                               "Average Reading Score":"{:,.2f}",
                               "% Passing Math":"{:,.2f}",
                               "% Passing Reading":"{:,.2f}",
                               'Overall Passing %':"{:,.2f}"
                                   })

print(school_data_complete["size"].max())
print(school_data_complete["size"].min())

# Perform the same operations as above, based on school size.

bins = [0, 999, 1999, 4999]
group_names = ["Small (<1000)", "Medium (1000-1999)", "Large (>2000)"]

school_data_complete["Size Summary"] = pd.cut(school_data_complete["size"], bins, labels=group_names, include_lowest=True)
group_by_size = school_data_complete.groupby("Size Summary")

# Average Math Score

average_math_ = group_by_size["math_score"].mean()

# Average Reading Score

average_reading_ = group_by_size["reading_score"].mean()

# % Passing Math

passing_math = school_data_complete.loc[school_data_complete["math_score"] >= 70].groupby("Size Summary")["Student ID"].count()/group_by_size['Student ID'].count()
passing_math_formatted = passing_math  * 100

# % Passing Reading

passing_reading = school_data_complete.loc[school_data_complete["reading_score"] >= 70].groupby("Size Summary")["Student ID"].count()/group_by_size['Student ID'].count()
passing_reading_formatted = passing_reading * 100

# Overall Passing Rate (Average of the above two)

overall = school_data_complete[(school_data_complete['reading_score'] >= 70) & (school_data_complete['math_score'] >= 70)].groupby('Size Summary')['Student ID'].count()/group_by_size['Student ID'].count()
overall_formatted = overall * 100

size_summary_df = pd.DataFrame({
                                   "Average Math Score": average_math_ ,
                                   "Average Reading Score": average_reading_,
                                   '% Passing Math': passing_math_formatted,
                                   '% Passing Reading': passing_reading_formatted,
                                   'Overall Passing %': overall_formatted
                                   })

size_summary_df.style.format({"Average Math Score":"{:,.2f}",
                               "Average Reading Score":"{:,.2f}",
                               "% Passing Math":"{:,.2f}",
                               "% Passing Reading":"{:,.2f}",
                               'Overall Passing %':"{:,.2f}"
                                   })

# Perform the same operations as above, based on school type

grouped_by_type = school_data_complete.groupby('type')

# Average Math Score

average_math_ = grouped_by_type["math_score"].mean()

# Average Reading Score

average_reading_ = grouped_by_type["reading_score"].mean()

# % Passing Math

passing_math = school_data_complete.loc[school_data_complete["math_score"] >= 70].groupby("type")["Student ID"].count()/grouped_by_type['Student ID'].count()
passing_math_formatted = passing_math  * 100

# % Passing Reading

passing_reading = school_data_complete.loc[school_data_complete["reading_score"] >= 70].groupby("type")["Student ID"].count()/grouped_by_type['Student ID'].count()
passing_reading_formatted = passing_reading * 100

# Overall Passing Rate (Average of the above two)

overall = school_data_complete[(school_data_complete['reading_score'] >= 70) & (school_data_complete['math_score'] >= 70)].groupby('type')['Student ID'].count()/grouped_by_type['Student ID'].count()
overall_formatted = overall * 100

type_summary_df = pd.DataFrame({
                                   "Average Math Score": average_math_ ,
                                   "Average Reading Score": average_reading_,
                                   '% Passing Math': passing_math_formatted,
                                   '% Passing Reading': passing_reading_formatted,
                                   'Overall Passing %': overall_formatted
                                   })

type_summary_df.style.format({"Average Math Score":"{:,.2f}",
                               "Average Reading Score":"{:,.2f}",
                               "% Passing Math":"{:,.2f}",
                               "% Passing Reading":"{:,.2f}",
                               'Overall Passing %':"{:,.2f}"
                                   })
