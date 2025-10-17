# This project is using Linux cmd to process and analyze data from Movies.csv file 

So the important point in this project use with all cmd Linux no tool, package support



## 1. Sort Movies by Release Date (Month/Day)
- Keep only rows where the release date exists ($16 != ""). Extract month and day from $16, then sort by:
Year ($19) descending
Month descending
Day descending
- Command:
	awk -F";" '
	$16 != "" {
  	OFS=";";
  	split($16, date_parts, "/");
  	sortable_month = sprintf("%02d", date_parts[1]);
  	sortable_day = sprintf("%02d", date_parts[2]);
  	print $6, $16, $19, sortable_month, sortable_day
	}
	' movies_22.csv |
	sort -t';' -k3,3nr -k4,4nr -k5,5nr |
	cut -d';' -f1,2 > cau1.csv
## 2. Filter Movies with Rating > 7.5
- Goal:
	List movie names and ratings above 7.5, sorted by rating descending.
- Command:
	awk -F';' 'BEGIN{OFS=";"} $18 > 7.5 {print $6, $18}' movies_22.csv |
	sort -t';' -k2,2nr > cau2_sorted.csv
## 3. Find Highest and Lowest Revenue Movies
Extract revenue ($5) and movie name ($6), then find the highest and lowest revenue.
- Command:
	awk -F';' '{print $5, $6}' movies_22.csv > cau3.csv
- Highest revenue:
	sort -t, -k1,1nr cau3.csv | head -n 1 >> cau3.csv
- Lowest revenue:
	sort -t, -k1,1n cau3.csv | head -n 1 >> cau3.csv
## 4. Calculate Total Revenue
- Goal:
	Compute the total revenue from all movies.
- Command:
	awk -F';' '{sum += $5} END {print "Total revenue:", sum}' movies_22.csv > cau4.csv
## 5. Top 10 Movies by Revenue
- Goal:
	Sort movies by revenue and get the top 10.
- Command:
	awk -F';' '{print $5, $6}' movies_22.csv |
	sort -t, -k1,1nr |
	head -n 10 > cau5.csv
## 6. Most Frequent Director and Cast
- Goal:
	Find which director and which cast member appear most often.
- Command:
- Extract director and cast info
	awk -F';' '{print $2, $3}' movies_22.csv > cau6.csv
- Most frequent director
	awk -F';' '{print $2}' cau6.csv | sort | uniq -c | sort -nr | head -n 1
- Most frequent cast member
	awk -F';' '{print $1}' cau6.csv | tr '|' '\n' | sort | uniq -c | sort -nr | head -n 1
## 7. Count Movie Genres
- Goal:
Count how many movies belong to each genre.
- Command:
	awk -F';' '{print $14}' movies_22.csv |
	tr '|' '\n' |
	sort | uniq -c | sort -n > cau7.csv
