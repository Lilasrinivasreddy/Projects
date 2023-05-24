SELECT
  result_id,
  id,
  status
FROM
  (
    SELECT
      result_id,
      id,
      status,
      MAX(date1) AS latest_date
    FROM
      Tapsium_AIML.dummy_results_data
    GROUP BY
      result_id,
      status
  ) AS latest_results
WHERE
  status IN ('failure', 'success')
ORDER BY
  latest_date DESC;
