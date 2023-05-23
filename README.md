UPDATE
  Tapsium_AIML.dummy_results_data
SET
  date1 = (
    SELECT
      MAX(date)
    FROM
      (
        SELECT
          result_id,
          date,
          status,
          ROW_NUMBER() OVER (PARTITION BY status ORDER BY date DESC) AS rn
        FROM
          Tapsium_AIML.dummy_results_data
        LEFT JOIN
          Tapsium_AIML.new_results_data
          ON
            Tapsium_AIML.dummy_results_data.result_id = Tapsium_AIML.new_results_data.result_id
      ) AS t
    WHERE
      rn = 1
  )
WHERE
  status IN ('failure', 'success');
