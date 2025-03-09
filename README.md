Below are some queries I wrote to pull data from SQL

/* Joing two tables to retrieve required data*/


SELECT 
       pmv.[City/Town],
	   pmv.[Postcode],
	   pmv.[Suburb],
	   pmv.[City/Town],
	   aps.[Postcode],
	   aps.[Suburb],
	   pmv.[Property_Median_Value],
	   lat,
	   lon
FROM [dbo].[NSW_PropertyMedainValue] pmv
	INNER JOIN [dbo].[AUS_Post_suburb] aps
	 ON pmv.[City/Town] =aps.city
	  AND pmv.Suburb = aps.suburb
	 AND pmv.Postcode = aps.postcode
WHERE Property_Median_Value BETWEEN 0 and 750000
and [City/Town] <> '#N/A'

/* Number of cities in each state*/


SELECT state,
       Count(DISTINCT city) AS NumberOfCities
FROM   dbo.aus_post_suburb
GROUP  BY state
ORDER  BY state;


/* Number of unique postcodes, and suburbs in each city */


SELECT city,
       Count(DISTINCT postcode) AS UniquePostcodes,
       Count(DISTINCT suburb)   AS UniqueSuburbs
FROM   aus_post_suburb
GROUP  BY city; 


/* The Average Property Median Value by Suburb, and by Postcode separately, 
   and then by Suburb and Postcode together. 
   Then Filtered to remove those records where there is no median value. */

   
SELECT
	  Postcode,
	  AVG(Property_Median_Value) AS AveragePropertyValue
FROM 
	[dbo].[NSW_PropertyMedainValue]
WHERE
    Property_Median_Value IS NOT NULL
GROUP BY 
	    Postcode

SELECT
	  Suburb,
	  AVG(Property_Median_Value) AS AveragePropertyValue
FROM 
	[dbo].[NSW_PropertyMedainValue]
WHERE
    Property_Median_Value IS NOT NULL
GROUP BY 
	    Suburb


SELECT
	  Suburb,
	  Postcode,
	  AVG(Property_Median_Value) AS AveragePropertyValue
FROM 
	[dbo].[NSW_PropertyMedainValue]
WHERE
    Property_Median_Value IS NOT NULL
GROUP BY 
	    Suburb,
		Postcode
