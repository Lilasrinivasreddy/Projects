SELECT result_id, status
FROM (
  SELECT
    nd.result_id,
    nd.status,
    nd.date1,
    ROW_NUMBER() OVER (PARTITION BY nd.status ORDER BY nd.date1 DESC) AS rn
  FROM (
    SELECT
      d.result_id,
      n.status,
      d.date1
    FROM
      Tapsium_AIML.dummy_results_data AS d
      JOIN Tapsium_AIML.new_results_data AS n ON d.result_id = n.result_id
    WHERE
      n.status IN ('failure', 'success')
  ) AS nd
) AS result
WHERE rn = 1;
