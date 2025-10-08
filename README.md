#Syntax i use to let you have a good view at my code
1. I don't reformat type of datetime, take the release_year merge with day and month in release_day and then > into csv file 
awk -F";" '
$16 != "" {
OFS=";"
split($16, date_parts, "/");
sortable_month = sprintf("%02d", date_parts[1]);
sortable_day = sprintf("%02d", date_parts[2]);
print $6, $16, $19, sortable_month, sortable_day
}
' movies_22.csv | sort -t';' -k3,3nr -k4,4nr -k5,5nr | cut -d';' -f1,2 > cau1.csv
2. awk -F';' 'BEGIN{OFS=";"} $18 > 7.5 {print $6, $18}' movies_22.csv | sort -t';' -k2,2nr > cau2_sorted.csv
3. awk -F';' '{print $5, $6}' movies_22.csv > cau3.csv
highest: sort -t, -k1,1nr cau3.csv | head -n 1
lowest: sort -t, -k1,1n cau3.csv | head -n 1
then >> again in cau3.csv
4. awk -F';' '{sum += $5} END {print "Total revenue:", sum}' movies_22.csv > cau4.csv
5. awk -F';' '{print $5, $6}' movies_22.csv | sort -t, -k1,1nr | head -n 10
6. take director and cast to cau6.csv then >> again
Director: awk -F';' '{print $2}' cau6.csv | sort | uniq -c | sort -nr | head -n 1
Cast: awk -F';' '{print $1}' cau6.csv | tr '|' '\n' | sort | uniq -c | sort -nr | head -n 1
7. awk -F';' '{print $14}' movies_22.csv | tr '|' '\n' | sort | uniq -c | sort -n > cau7.csv 
