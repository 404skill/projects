# Borrowing & Returning Items

## Overview  
   Now that you can create and store both **users** and **items**, the next step is to implement the **core library functionality**: borrowing and returning. This is where you enforce membership limits, track which items are on loan, and manage due dates or borrowing durations (if your design includes them).  

## Membership Limits  
   Recall that each membership type has different borrowing limits for books, magazines, DVDs, and (later) CDs. For example:
   - **Student**: Up to 5 books, 5 magazines, 2 DVDs  
   - **Basic**: Up to 3 books, 3 magazines, 2 DVDs  
   - **Premium**: Up to 10 books, 5 magazines, 5 DVDs  

   In subsequent steps, you’ll add CDs with their own limits.  

## Data Model Considerations  
   - **User**:  
     - Already has `membership_type`.  
     - You may want a convenient way to check how many items of each type the user currently holds.  
   - **Item**:  
     - Has `status`, which can be `"available"` or `"borrowed"`.  
   - **Borrowing Records** (optional):  
     - To track the date/time of borrowing, due dates, or return dates.  
     - This can be a dedicated SQL table (e.g., `borrowings`) or a sub-array of “history” within the user record, or both—depending on your design.

## Borrowing an Item

   **Endpoint**: `POST /items/:id/borrow`  

   **Request Body** (example):
   ```json
   {
     "user_id": "123e4567-e89b-12d3-a456-426614174000",
     "item_type": "book"
   }
   ```
   **Implementation Steps**:
   1. **Validate the user**: Confirm the `user_id` exists.  
   2. **Check membership limits**: How many items of this type does the user currently hold? Compare to allowed limit.  
   3. **Check the item**: Verify that the item with `:id` is found, and that `type` matches `"book"` (or user’s requested item type).  
   4. **Check item availability**: Is `status` `"available"`? If not, return an error (e.g., 400 or 409).  
   5. **Borrow**:  
      - Update the item’s `status` to `"borrowed"`.  
      - Optionally store a “borrowed_date” somewhere (item record or a separate “borrowing history” table/array).  
   6. **Respond** with 200 or 201 status code and a message indicating success (and possibly the new due date or borrowed date).

   **Error Handling** Examples:
   - 400 Bad Request if membership limit reached or item is already borrowed.  
   - 404 Not Found if user or item doesn’t exist.

## Returning an Item

   **Endpoint**: `POST /items/:id/return`  

   **Request Body** (example):
   ```json
   {
     "user_id": "123e4567-e89b-12d3-a456-426614174000"
   }
   ```
   **Implementation Steps**:
   1. **Validate the user**: Confirm the `user_id` exists.  
   2. **Check the item**: Ensure the item with `:id` is found and is actually `borrowed`.  
   3. **Confirm who borrowed it**: Optionally verify that the item is borrowed by this specific user (if you’re tracking that detail).  
   4. **Return**:  
      - Update `status` to `"available"`.  
      - Store a “returned_date” in the borrowing record or history.  
   5. **Respond** with a success code (200 OK).

   **Error Handling** Examples:
   - 400 Bad Request or 409 if item is not currently borrowed.  
   - 404 Not Found if user or item doesn’t exist.

## Tracking Borrowing History  
   - **Goal**: Keep a record of all items a user has borrowed, including borrowed date and return date.  
   - **Possible Approaches**:  
     1. **Dedicated “History” Table**: Each row represents one borrow-return cycle (with `borrowed_date` and `returned_date`).  
     2. **Embedded Array in User**: For each borrow event, push an object into the user’s `history`. Upon return, update that object’s `returned_date`.  
   - **GET /users/:id/history** Example Response:
     ```json
     [
       {
         "item_id": "abc123",
         "item_type": "book",
         "borrowed_date": "2023-01-01",
         "returned_date": "2023-02-01"
       },
       {
         "item_id": "xyz789",
         "item_type": "dvd",
         "borrowed_date": "2023-03-10",
         "returned_date": null
       }
     ]
     ```

## Due Dates & Borrow Durations (Optional)  
   - Books: up to 3 months  
   - Magazines: 1 month  
   - DVDs: 1 month  
   - This is optional if you just want to track availability. But if you do, you might store a `due_date` during the borrow step and check if it’s overdue on return.  

## Practical Tips  
   - **Updating Limits**: You might keep a count of how many items each user currently has borrowed per item type. This can be calculated in real-time from a “borrowings” table or stored as fields in the user record and updated each time something is borrowed or returned.  
   - **Conflicts**: If multiple users try to borrow the same item at once, ensure your code handles concurrency gracefully (if your project context requires it).  

## Testing Your Borrow/Return Logic  
   - Create a few users with different membership types.  
   - Create multiple items (books, magazines, DVDs).  
   - Attempt to borrow items until you hit the membership limit—ensure your API returns an error.  
   - Return items and verify they become available again.  
   - (Optional) Log or display due dates if you’re implementing them.  

## Next Steps  
   With borrowing and returning logic in place, you have the **foundation** for a functioning library system. The next pages will cover:  
   - **Adding a new item type** (CDs).  
   - **Promotion System** that doubles book limits for power users (who borrow 15+ books).  
   - **Fines, Overdue Handling,** or other advanced features (optional).  

By the end of this section, your library should be able to handle the **core workflow** of checking out and returning library materials, respecting membership limits and item availability.  