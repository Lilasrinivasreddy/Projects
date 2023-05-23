SELECT result_id, status
FROM (
  SELECT
    CASE
      WHEN n.status = 'failure' THEN d.id
      WHEN n.status = 'success' THEN d.id
    END AS result_id,
    n.status,
    ROW_NUMBER() OVER (ORDER BY d.date1 DESC) AS row_num
  FROM Tapsium_AIML.dummy_results_data d
  JOIN Tapsium_AIML.new_results_data n ON d.result_id = n.result_id
  WHERE
    (n.status = 'failure' AND d.date1 = (SELECT MAX(date1) FROM Tapsium_AIML.dummy_results_data WHERE result_id = n.result_id))
    OR
    (n.status = 'success' AND d.date1 = (SELECT MAX(date1) FROM Tapsium_AIML.dummy_results_data WHERE result_id = n.result_id))
) subquery
WHERE row_num <= 3;
