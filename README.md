# Capstone-Project-Files
Data and Source Code
/* Generated Code (IMPORT) */
/* Source File: Hotel Reservations_Transformed data.csv */
/* Source Path: /home/u60105201/sasuser.v94/Capstone */
/* Code generated on: 4/6/23, 8:16 PM */
/* Upload Data file
%web_drop_table(WORK.IMPORT);


FILENAME REFFILE '/home/u60105201/sasuser.v94/Capstone/Hotel Reservations_Transformed data.csv';

PROC IMPORT DATAFILE=REFFILE
	DBMS=CSV
	OUT=WORK.IMPORT;
	GETNAMES=YES;
RUN;

PROC CONTENTS DATA=WORK.IMPORT; RUN;
/* Run Summary Statistics
proc means data=WORK.IMPORT chartype mean std min max median n vardef=df 
		qmethod=os;
	var no_of_adults no_of_children no_of_weekend_nights no_of_week_nights 
		type_of_meal_plan_id required_car_parking_space lead_time arrival_year 
		arrival_month_part_id Arrival_DOW market_segment_type_id repeated_guest 
		no_of_previous_cancellations no_of_previous_bookings_not_canc 
		avg_price_per_room no_of_special_requests;
	class booking_status booking_status_id;
run;
/* Run Correlation Analysis
proc corr data=WORK.IMPORT pearson nosimple outp=work.Corr_stats plots=matrix;
	var booking_status_id;
	with no_of_adults no_of_children no_of_weekend_nights no_of_week_nights 
		type_of_meal_plan_id required_car_parking_space room_type_reserved_id 
		lead_time arrival_month_part_id Arrival_DOW market_segment_type_id 
		repeated_guest no_of_previous_cancellations no_of_previous_bookings_not_canc 
		avg_price_per_room no_of_special_requests;
run;
/* Run Logistic Regression
proc logistic data=WORK.IMPORT plots=(roc);
	model booking_status_id(event='1')=lead_time repeated_guest avg_price_per_room 
		no_of_special_requests no_of_adults no_of_children no_of_weekend_nights 
		no_of_week_nights type_of_meal_plan_id required_car_parking_space 
		room_type_reserved_id arrival_year arrival_month_part_id 
		arrival_date_daypart_ID market_segment_type_id no_of_previous_cancellations 
		Arrival_DOW no_of_previous_bookings_not_canc / link=logit ctable 
		selection=stepwise slentry=0.05 slstay=0.05 hierarchy=single technique=fisher;
run;
