#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// A job has start time, finish time and profit.
struct Job
{
	int start, finish, profit;
};

// A utility function that is used for sorting events
// according to finish time
bool jobComparator(Job s1, Job s2)
{
	return (s1.finish < s2.finish);
}

// Find the latest job (in sorted array) that doesn't
// conflict with the job[i]. If there is no compatible job,
// then it returns -1.
int latestNonConflict(vector<Job>& jobs, int i)
{
	for (int j=i-1; j>=0; j--)
	{
		if (jobs[j].finish <= jobs[i-1].start)
			return j;
	}
	return -1;
}

// A recursive function that returns the maximum possible
// profit from given array of jobs. The array of jobs must
// be sorted according to finish time.
int findMaxProfitRec(vector<Job>& jobs, int n)
{
	// Base case
	if (n == 1) return jobs[n-1].profit;

	// Find profit when current job is included
	int inclProf = jobs[n-1].profit;
	int i = latestNonConflict(jobs, n);
	if (i != -1)
	inclProf += findMaxProfitRec(jobs, i+1);

	// Find profit when current job is excluded
	int exclProf = findMaxProfitRec(jobs, n-1);

	return max(inclProf, exclProf);
}

// The main function that returns the maximum possible
// profit from given array of jobs
int findMaxProfit(vector<Job>& jobs, int n)
{
	// Sort jobs according to finish time
	sort(jobs.begin(), jobs.end(), jobComparator);
	return findMaxProfitRec(jobs, n);
}

// Driver program
int main()
{
    int numJobs;
    cout << "Enter the number of jobs: ";
    cin >> numJobs;

     vector<Job> jobs(numJobs);
     cout << "Enter job details (start, finish, profit):\n";
    for (int i = 0; i < numJobs; i++) {
         cin >> jobs[i].start >> jobs[i].finish >> jobs[i].profit;
    }
	
	cout << "The optimal profit is " << findMaxProfit(jobs, numJobs);
	return 0;
}