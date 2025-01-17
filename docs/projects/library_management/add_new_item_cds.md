# Adding Music CDs

## Overview
   By now, your system can handle **books**, **magazines**, and **DVDs**. In this section, you’ll **extend** your library catalog by introducing **music CDs** as a new item type. You’ll also update membership rules so that each user type can borrow a certain number of CDs.

## New Item Type: “CD”  
   - **Borrow Duration** (if you’re tracking): Up to 2 months.  
   - **Item Model**: Same structure as your existing items (`item_id`, `name`, `type`, `status`). You simply add `"cd"` as an allowed type.

## Membership Limits for CDs  
   - **Student**: Up to 2 CDs  
   - **Basic**: Up to 2 CDs  
   - **Premium**: Up to 4 CDs  
   Make sure you **enforce** these limits the same way you do for books, magazines, and DVDs.

## API Changes  

   - **POST /items**  
     - Accept `"cd"` in the `type` field.  
     - Example:
       ```json
       {
         "name": "Greatest Hits by Queen",
         "type": "cd"
       }
       ```
     - Store and return the item with `status = "available"` by default.

   - **Borrowing and Returning**  
     - Update any logic that checks membership limits so it includes CDs.  
     - Example: A Student can now hold 2 CDs at a time. If they already have 2 CDs borrowed, borrowing another CD should return a 400 Bad Request.

   - **Borrow Duration** (Optional)
     - If you’re tracking durations, store a `due_date` for CDs that’s 2 months from the borrowing date.

## Implementation Tips  
   - In your **borrow logic**, when a user requests a `"cd"`, your code should:
     1. Confirm user hasn’t hit the CD borrowing limit.  
     2. Verify the item is actually `"cd"` and is `"available"`.  
     3. Mark it as `"borrowed"`, update user’s borrowed count or borrowing record.  
   - In your **return logic**, ensure the item type `"cd"` is handled exactly like the others—just update the item’s status to `"available"` and record a return date if you’re tracking it.

## Testing with CDs  
   - **Create multiple CD items** via `POST /items` with `"type": "cd"`.  
   - **Borrow them** from different users (Student, Basic, Premium) to confirm each membership type’s new limit is enforced.  
   - **Try exceeding the CD limit** to ensure your system returns an appropriate error.  
   - **Return** CDs to verify items become available again.

## Optional Additional Features  
   - **Genre or Artist Field**: Add fields like `artist` or `genre` if you want to practice more complex item attributes.  
   - **Extended Validation**: Maybe disallow empty `name` or enforce a pattern for `artist`.

## Next Steps
   With CDs integrated, your Library Management System now supports:
   - Multiple item types (books, magazines, DVDs, CDs).  
   - Different membership limits for each type.  
   - Borrowing and returning logic for all items.  

   The **final** major feature (in the core specification) is the **Promotion System**—which allows users who have borrowed 15+ books in the last year to double their book limit. Move on to the **next section** to implement promotions and see how it integrates with your existing borrowing logic.