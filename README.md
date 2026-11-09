# Learning Practice: Experience-Based Testing

I tested the North Vacation Booking app (`airbnb-clone-ts`). I picked one feature: creating a booking. I used what I know from other booking websites and what I saw while clicking around in this app.

## How to run

Open a terminal in the `airbnb-clone-ts` folder:

```bash
npm install
```

Start the local data server on port 3001:

```bash
npx json-server --watch db.json --port 3001
```

Open a second terminal in the same folder:

```bash
npm run dev
```

Then open [http://localhost:5173/](http://localhost:5173/) in the browser.

## Step 1: Error Guessing

These are guesses based on other websites I have used. I do not have to prove they are bugs yet.

1. I think I might be able to confirm a booking without picking a date.
2. I think a new booking might go away if I refresh the page.
3. I think I might be able to book a date that already passed.

## Step 2: Exploratory Testing

Testing charter: I want to see if I can create a booking and find it on the Bookings page, and what happens if I leave a field empty or type a weird value.

| What I Tried | What Happened | What I Decided to Try Next |
| --- | --- | --- |
| Opened Home and then selected Add Booking | Home showed rental cards, including Cozy Cabin. The nav links were Home, Add Booking, and Bookings. The form heading was Book a Stay. The fields were Rental, Date, and Number of Guests. The button was Confirm Booking. There was only one date field. | I tried to submit the form with an empty date. |
| Left the date empty and selected Confirm Booking | I stayed on the form. The Date field became focused. I did not see an error message I could copy. | I tried a normal booking with all fields filled in. |
| Chose Cozy Cabin, set the date to 2026-12-15, set guests to 2, and selected Confirm Booking | I stayed on the same page. The form still showed those values. There was no success message. | I opened the Bookings page to see if the booking was saved. |
| Opened the Bookings page | I saw this text: Rental: Cozy Cabin Date: 2026-12-15 Guests: 2 | I refreshed the page to see if the booking would stay. |
| Refreshed the Bookings page | The list was empty. The booking was gone. | I wanted to see what is saved if I do not click the rental list. |
| Went back to Home so rentals loaded, opened Add Booking, did not click the rental list, set the date to 2026-12-20 and guests to 1, and submitted | The rental list looked like Cozy Cabin was already showing. After submit, Bookings showed: Rental: Date: 2026-12-20 Guests: 1. The rental name was blank. | I tried opening Add Booking without visiting Home first. |
| Opened a new tab and went straight to the add booking page | The rental list had 0 options. I submitted date 2026-12-22 and 1 guest. Bookings in that tab showed: Rental: Date: 2026-12-22 Guests: 1 | I tried 0 guests, a very large guest number, and a past date. |
| Set guests to 0, chose Cozy Cabin, set the date to 2026-12-25, and selected Confirm Booking | I stayed on the form with those values. The Bookings list did not show a 2026-12-25 booking. | I tried a very large guest number. |
| Set guests to 999, chose Cozy Cabin, set the date to 2026-12-26, and selected Confirm Booking | The Bookings list showed: Rental: Cozy Cabin Date: 2026-12-26 Guests: 999 | I tried a date in the past. |
| Chose Cozy Cabin, set the date to 2020-01-01, set guests to 2, and selected Confirm Booking | The Bookings list showed: Rental: Cozy Cabin Date: 2020-01-01 Guests: 2 | A later idea is to look at why there is only one date field and no check-in or check-out. |

## Step 3: Checklist-Based Testing

I made a list of checks and marked each one Passed, Failed, or Unable to Test.

| Check | Result | Notes |
| --- | --- | --- |
| The form does not add a booking when the date is empty | Passed | Confirm Booking left me on the form and focused the Date field. I did not see a written error message. |
| A complete booking appears on the Bookings page | Passed | I saw Rental: Cozy Cabin Date: 2026-12-15 Guests: 2 |
| A booking is still on the Bookings page after I refresh | Failed | After I refreshed, the list was empty |
| If I do not click the rental list, the booking still saves the rental name that was showing | Failed | The list showed a blank rental name: Rental: Date: 2026-12-20 Guests: 1 |
| I can open Add Booking without visiting Home first and still see rentals to choose | Failed | The rental list had 0 options |
| The form does not accept 0 guests | Passed | No booking with date 2026-12-25 appeared in the list |
| The form does not accept a past date | Failed | The list showed Rental: Cozy Cabin Date: 2020-01-01 Guests: 2 |

## Step 4: Acceptance Testing

This part is from the user's side. Given is where I start. When is what I do. Then is what should happen.

| Given, When, Then | Result | Notes |
| --- | --- | --- |
| Given I visited Home and I am on the booking form, when I submit Cozy Cabin, the date 2026-12-15, and 2 guests, then the reservation should appear in my list of bookings | Passed | Bookings showed Rental: Cozy Cabin Date: 2026-12-15 Guests: 2 |
| Given I just created a booking, when I refresh the Bookings page, then I should still see that booking | Failed | After I refreshed, the list was empty |
| Given I am on the booking form, when I select Confirm Booking with an empty date, then the app should not add a booking | Passed | I stayed on the form and the Date field became focused |
| Given I am on the booking form and a rental name is visible, when I enter a date and guests without clicking the rental list and I submit, then the booking should keep that rental name | Failed | Bookings showed a blank rental name for date 2026-12-20 |
| Given I submitted a booking, when the save finishes, then I should see a message that my booking was confirmed | Failed | There was no confirmation message |

## AI Transparency

AI was used to create part of this file and to clean up its writing and formatting.
