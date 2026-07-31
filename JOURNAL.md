## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/88

**Issue title:** POST /reviews endpoint has no test for when the profile has no ingested documents

**Issue selection reasoning:** The reason that I chose this issue is that coupled with it being a tier 1 issue, targetted for people who are contributing open source for the first time like me, and having a clear problem statement, this issue is a good starting point for me to get familiar with the codebase and contribute meaningfully. Since I want to further my understanding with my already existing knowledge of Python and FastAPI with it's application in a production environment, this issue is a good starting point. Additionally, in terms of scope fit, this issue is a good fit for me because it is a small, well-defined task that I believe I can complete within the given timeframe, if not even sooner. 

**Tier:** [X] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**

On the /reviews endpoint of PathReview, there is currently no test suite that handles the scenario where there is a user's profile but has no ingested documents. So, to handle this case, we need to define a test file called test_review_routes.py inside tests/unit. This test file will contain a test function that will simulate a POST request to the /reviews endpoint with a profile that has no ingested documents. The expected behavior is that the endpoint should return an adequate error message indicating that there are no documents available for review. A successful fix would ensure that this edge case is properly tested, improving the robustness of the application and preventing potential runtime errors when users with empty profiles attempt to access the /reviews endpoint. Additionally, paying attention to the api/routes/reviews.py file is crucial, as it contains the logic for handling the POST /reviews endpoint. 


**Branch name:** test/88-test-review-endpoint

**Setup confirmation:** [X] App runs locally at localhost:5173

**Cohort ledger:** [X] Issue added to cohort ledger

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [Commit Link](https://github.com/DavidRaet/pathreview/commit/3d487c5f21a62eef749b49fa26054b5dfa706278)

**Reproduction summary:**
Since this is primarily a testing issue, the reproduction step is to create a test case that simulates a POST request to the /reviews endpoint with a profile that has no ingested documents. The expected behavior is that the endpoint should return an error message indicating that there are no documents available for review.

**PLAN.md link:** [PLAN.md](https://github.com/DavidRaet/pathreview/blob/test/88-test-review-endpoint/PLAN.md)

**Blockers or open questions:**

The plan might include actions that seem to go beyond the scope of the issue, but because PLAN.md documents the lack of a guard clause in the api/routes/reviews.py file, will it be okay to still add a guard clause in the api/routes/reviews.py file to check if the profile has ingested documents before proceeding with the test creation? 

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
Implemented the route-level guard from PLAN.md's first bullet: `create_review_endpoint` in `api/routes/reviews.py` now queries `IngestedSource` for the given `profile_id` before creating a review, and returns a 400 with a warning log ("Profile has no ingested documents") if none exist, instead of silently creating a pending review.

**Next steps:**
Create `test_review_routes.py` per PLAN.md. Then, proceed to set up `TestReviewRoutes` with fixtures for profiles with/without ingested documents, and add `test_post_reviews_no_documents` to verify the 400 response and warning log.

**Blockers:**
N/A

---

### Check-in 2 (end of week)

**PR link:** [PR #88](https://github.com/ascherj/pathreview/pull/476)

**Branch:** test/88-test-review-endpoint

**What you built:**
For PathReview's `/reviews` endpoint, I implemented a guard clause in the `create_review_endpoint` function within `api/routes/reviews.py`. This guard checks if the profile has any ingested documents before proceeding to create a review. If no documents are found, it returns a 400 response with a warning log stating "Profile has no ingested documents". Additionally, I created a test file `test_review_routes.py` in `tests/integration` that contains a test case `test_post_reviews_no_documents`, which simulates a POST request to the `/reviews` endpoint with a profile that has no ingested documents, and confirms that the endpoint returns the expected 400 response and logs the appropriate warning message. 

**Tests added or updated:**
[Which test files did you touch? What do they cover?]

- `tests/integration/test_review_routes.py`: This test file was added and contains the test case `test_post_reviews_no_documents`, which runs a POST request to the `/reviews` endpoint, confirming that the endpoint returns a 400 response and logs the warning message "Profile has no ingested documents" when the profile has no ingested documents.


**Self-review confirmation:** [X] make check passes  [X] make test-unit passes

**Draft PR feedback received from:** none