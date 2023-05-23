SELECT
  result_id,
  status,
  date
FROM
  (
    SELECT
      result_id,
      status,
      date,
      ROW_NUMBER() OVER (PARTITION BY status ORDER BY date DESC) AS rn
    FROM
      Tapsium_AIML.dummy_results_data
    JOIN
      Tapsium_AIML.new_results_data
      ON dummy_results_data.result_id = new_results_data.result_id
  ) AS t
WHERE
  rn = 1;
