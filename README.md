SELECT
  CASE WHEN status = 'failure' THEN nd.result_id
       WHEN status = 'success' THEN sd.result_id
  END AS result_id,
  status
FROM
  (
    SELECT result_id, MAX(date1) AS max_date
    FROM Tapsium_AIML.dummy_results_data
    GROUP BY result_id
  ) AS nd
JOIN Tapsium_AIML.new_results_data AS sd ON nd.result_id = sd.result_id
WHERE
  (status = 'failure' AND nd.max_date = (SELECT MAX(date1) FROM Tapsium_AIML.dummy_results_data))
  OR
  (status = 'success' AND nd.max_date = (SELECT MAX(date1) FROM Tapsium_AIML.dummy_results_data));
