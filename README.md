# DB_Final
Establish a custom time series data similarity comparison function based on the timescaledb database system.

## Prerequisites
Before you begin, ensure you have met the following requirements:  

<pre><code>python >= 3.8
postgresql >= 14</code></pre>
Make sure your postgresql contain timescaledb extenstion.  

## Datas for demo
Please run the following query to create a hybertable to store the data.
<pre><code>DROP TABLE IF EXISTS company_stock_prices;

CREATE TABLE company_stock_prices (
    id SERIAL ,
    company TEXT NOT NULL,
    timestamp TIMESTAMPTZ NOT NULL,
    price NUMERIC NOT NULL
);

DROP TABLE IF EXISTS target_company_stock_prices;

CREATE TABLE target_company_stock_prices (
    id SERIAL ,
    company TEXT NOT NULL,
    timestamp TIMESTAMPTZ NOT NULL,
    price NUMERIC NOT NULL
);</code></pre>

After you make sure the table exsist. Please run the following command to write the csv files into database.
<pre><code>python write_datas.py
python write_inputdata.py</code></pre>

## Create custom function
Run the following command to create the time-series data similarity search function.
<pre><code>psql -U username -d database_name -a -f createfunction.sql
</code></pre>

## Example for the custom function
1. Pure Euclidean
<pre><code>psql -U username -d database_name -a -f appl_euclidean.sql  
psql -U username -d database_name -a -f wmt_euclidean.sql  
</code></pre>
2. V-shift Euclidean
<pre><code>psql -U username -d database_name -a -f gold_vshift_euclidean.sql  
psql -U username -d database_name -a -f fromch_vshift_euclidean.sql  </code></pre>
3. DTW (Dynamic Time Warping)
<pre><code>psql -U username -d database_name -a -f gold_war_dtw.sql</code></pre>
4. Cross correlation
<pre><code></code></pre>
The result will be write into the table named  
| Table Name               | company | min distance | best index | start time | end time |
|--------------------------|:-----:|:-----:|:-----:|:-----:|:-----:|
| wmt_euclidean_results    |   ✓   |    ✓  |       |       |       |
| appl_euclidean_results   |   ✓   |    ✓  |       |       |       |
| gold_vsift_results       |   ✓   |    ✓  |       |       |       |
| fromch_vsift_results     |   ✓   |    ✓  |       |       |       |
| gold_war_dtw_results     |   ✓   |    ✓  |    ✓  |    ✓  |    ✓  |
<!-- | Row 6                    | Data  | Data  | Data  | Data  | Data  | -->


## Output the result to csv
You can run the following python sql to output the table result to csv format.
<pre><code>psql -U username -d database_name -a -f output.sql</code></pre>
