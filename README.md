# Job
def job_selection(n, jobs):
    # Combine the job data and sort by end time
    job_data = []
    for i in range(n):
        start_time = int(jobs[3*i])
        end_time = int(jobs[3*i + 1])
        profit = int(jobs[3*i + 2])
        job_data.append((start_time, end_time, profit))
    job_data.sort(key=lambda x: x[1])

    # Use dynamic programming to find the optimal selection
    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        dp[i] = dp[i-1]
        for j in range(i-1, -1, -1):
            if job_data[j][1] <= job_data[i-1][0]:
                dp[i] = max(dp[i], dp[j] + job_data[i-1][2])
                break

    # Calculate the number of jobs left and earnings available for others
    max_profit = dp[n]
    jobs_left = n
    for i in range(n-1, -1, -1):
        if dp[i] == max_profit:
            jobs_left = n - i
        else:
            break
    other_earnings = sum([job_data[i][2] for i in range(n) if i < jobs_left])

    return [jobs_left, other_earnings]

# Example usage
n = int(input("Enter the number of Jobs\n"))
jobs = []
for i in range(n):
    print("Enter job start time, end time, and earnings")
    jobs.append(input())
    jobs.append(input())
    jobs.append(input())

result = job_selection(n, jobs)
print("The number of tasks and earnings available for others")
print("Task:", result[0])
print("Earnings:", result[1])
