# Promotion System

## Overview
   You’ve successfully built a system where users can borrow and return items (books, magazines, DVDs, and CDs) with membership-specific limits. Now, you’ll add a **promotion feature** that rewards avid readers by **doubling** their **book** borrowing limit if they meet certain criteria.

## Promotion Rules  
   - If a user has borrowed **15 or more books in the last year**, their book borrowing limit is **doubled**.  
   - This applies to all membership types. For example:
     - A Student can normally hold 5 books. If they’re promotion-eligible, they can hold **10** books.  
     - A Premium member with a 10-book limit can borrow **20** books if they qualify.  

## Defining Promotion Eligibility  
   - **Time Window**: Count how many books were borrowed in the **past 12 months** (or 365 days from now).  
   - **Threshold**: If borrowed `>= 15` books in that period, set a “promotion-eligible” flag for the user.  

   **Implementation Approaches**  
   - **On-The-Fly Check**:  
     1. Each time a user attempts to borrow a **book**, look at their borrowing history for the last year.  
     2. If it’s 15 or more, consider them promotion-eligible right away.  
   - **Cached/Stored Flag**:  
     1. When they borrow a book, calculate if they’ve reached the threshold.  
     2. Store a boolean (e.g., `isPromotionEligible`) on the user record.  
     3. Update it periodically or whenever a borrow action happens.

## Borrowing Logic
   - When a user attempts to borrow a **book**:
     1. Check how many books they’ve borrowed in the past 12 months.  
     2. If **>= 15**, override the standard membership limit with the **double** limit.  
     3. Continue normal checks (e.g., item availability).  

   **Example**:  
   - Student limit for books = 5.  
   - With promotion, limit = 10.  
   - If the user is promotion-eligible, treat their book limit as 10 instead of 5.

## Implementation Details  
   - **Borrowing History** is crucial. You need to store not just *whether* they borrowed items, but also *when*.  
   - If you’re using a dedicated “borrowings” table, you might run a query like:
     ```sql
     SELECT COUNT(*) 
       FROM borrowings
      WHERE user_id = :userId
        AND item_type = 'book'
        AND borrowed_date >= :oneYearAgo
     ```
     If the result is `>= 15`, they’re eligible.  
   - If you’re storing history in-memory or in a JSON file, loop through the user’s history entries, filter out those older than a year, and count how many are `"book"`.

## Endpoints (Optional Enhancements)  
   - **GET /users/:id/promotion-status**  
     - Returns an object indicating if the user is promotion-eligible and (if yes) what their new limit is.  
     - Example Response:
       ```json
       {
         "user_id": "123e4567-e89b-12d3-a456-426614174000",
         "promotion_eligible": true,
         "effective_book_limit": 10
       }
       ```
   - **PUT /users/:id/promotion-reset** (completely optional)  
     - If you want an admin to manually reset or revoke the promotion status for any reason.

## Testing Promotion Logic
   - Create a user and borrow **15 books** (you can do this quickly by auto-generating the borrow history if you want).  
   - Attempt to borrow a **16th** book:
     - Without the promotion, the user would normally be blocked if their limit is 5 or 10.  
     - With the promotion in effect, they should be allowed to exceed the normal membership cap.  
   - Ensure that the rule only applies to **books** (magazines, DVDs, and CDs remain unchanged).

## Performance Considerations (for the curious)  
   - If you’re frequently checking large borrowing histories, you might need indexing or caching.  
   - For this project, a straightforward solution (counting rows or iterating over an array) is typically fine.

## Next Steps
   - With promotions in place, you have a **fully-featured** system covering user creation, item storage, borrowing/returning, and specialized membership logic.  
   - You can now:
     - Add **fines**, **overdue fees**, or **notifications**.  
     - Improve **search** functionality (e.g., find items by name).  
     - Implement **authentication** if you want to secure these endpoints.  

**Congratulations!**  
You’ve built a robust **Library Management System** that demonstrates real-world backend challenges—database design, membership logic, containerization, and advanced features like promotions. Whether you stop here or keep adding more features, you’ve laid a solid foundation.