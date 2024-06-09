
# Insurance Analysis using MySQL

- This project focuses on harnessing the power of MySQL for in-depth insurance analytics, specifically tailored for risk assessment and coverage optimization. Leveraging a dataset comprising policyholder information, claim history, and relevant demographics, the project aims to extract actionable insights to inform insurance strategies and decision-making processes.

- Furthermore, I effectively communicated the data insights in a structured and coherent manner, enabling sales professionals to pinpoint key areas for enhancement and enact focused strategies. This project underscores my adeptness in MySQL data analysis and my capacity to offer actionable suggestions for optimizing car sales.

- By undertaking this insurance project, I have showcased my proficiency in data analysis, empowering enterprises with evidence-based insights to elevate sales performance, thereby bolstering the success of sales teams and fostering overall company expansion.

### 1.List all columns for all patients.
````sql
select * from insurance_data;
````
### 2.Display the average claim amount for patients in each region.
````sql
select region,avg(claim) as avg_claim from insurance_data group by region;
````
### 3.List the maximum and minimum BMI values in the table.
````sql
select max(bmi) as max_bmi,min(bmi) as min_bmi from insurance_data;
````
### 4.List the PatientID, age, and BMI for patients with a BMI between 40 and 50.
````sql
select PatientID,age,bmi from insurance_data where bmi between 40 and 50;
````
### 5.Select the number of smokers in each region.
````sql
select region,count(PatientID) from insurance_data where smoker="Yes"
group by region;
````
### 6.What is the average claim amount for patients who are both diabetic and smokers?
````sql
select avg(claim) as avg_claim from insurance_data where diabetic="Yes" and smoker="Yes";
````
### 7.Retrieve all patients who have a BMI greater than the average BMI of patients who are smokers.
````sql
select * from insurance_data where smoker="Yes" and  bmi > (select avg(bmi) from insurance_data where smoker="Yes");
select avg(bmi) from insurance_data where smoker="Yes";
````
### 8.List the average claim amount for patients in each age group.
````sql
select 
	case when age < 18 then "Under 18"
    when age between 18 and 30 then "18-30"
    when age between 31 and 50 then "31-50"
    else "Over 50"
end as age_group,
round(avg(claim),2) as avg_claim
from insurance_data
group by age_group;
````
### 9.Retrieve the total claim amount for each patient, along with the average claim amount across all patients.
````sql
select *,sum(claim) over(partition by PatientID) as total_claim,avg(claim) over() as avg_claim from insurance_data;
````
### 10.Retrieve the top 3 patients with the highest claim amount, along with their respective claim amounts and the total claim amount for all patients.
````sql
select PatientID, claim,sum(claim) over() as total_claim from insurance_data
order by claim desc limit 3;
````
### 11.List the details of patients who have a claim amount greater than the average claim amount for their region.
````sql
select * from insurance_data t1 
where claim > (select avg(claim) from insurance_data t2 where t2.region = t1.region);
select * from (select *, avg(claim)  over(partition by region) 
as avg_claim from insurance_data) as subquery
where claim > avg_claim;
````
### 12.Retrieve the rank of each patient based on their claim amount.
````sql
select * , rank() over(order by claim desc) from insurance_data;
````
### 13.List the details of patients along with their claim amount and their rank based on claim amount within their region.
````sql
select * , rank() over(order by claim desc) from insurance_data;
select *, rank() over(partition by region order by claim desc) from insurance_data;
````
