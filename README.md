# feat-calendar-v2

https://github.com/user-attachments/assets/d868b68b-cbe0-4e5a-bda6-fb27c387e853

![image](https://github.com/user-attachments/assets/acb69697-49f8-472a-82cf-028d4ecf4d6e)
![image](https://github.com/user-attachments/assets/df309ef9-c2c1-4aee-a8fc-8513d6cceee3)
![image](https://github.com/user-attachments/assets/6abbf6ba-f687-41e0-9855-fb6c79606851)


### 👉🏽  Overview:

🔗REFERENCE FEATURES  - [Demos, Examples of Syncfusion React UI Components](https://ej2.syncfusion.com/react/demos/?_gl=1*si0su6*_gcl_au*Mzc5MjM1MTI5LjE3MTgxMzYxNzU.*_ga*NjI0NDgxNjE2LjE3MTgxMzYxNzU.*_ga_41J4HFMX1J*MTcyMjE4NTEyMS44LjEuMTcyMjE4NjkzNC4wLjAuMA..#/bootstrap5-dark/schedule/timeline-resources)

Make A React Scheduler component(Custom/Library) exactly like the **shared video** and functionality should exactly same as https://www.syncfusion.com/react-components/react-scheduler
Any changes made to the calendar from other components should return the updated data in the following format:
```json
{
  "id": "user1",
  "exerciseName": "Bicep & Tricep",
  "trainerName": "Raj",
  "startTime": "13:12",
  "endTime": "14:12",
  "period": "PM",
  "customerName": "John",
  "currentDate": "2024-07-08T05:42:09.599Z",
  "userPicture": "/default-user-1.jpg"
}
```
### 👉🏽 Task Description
### Requirements: 
1. **Framework & Tools**: (Don't use other tools or frameworks)
   - **Next.js**: Use the App Router without the src folder structure. (Typescript)
   - **Redux**: Central state management.
   - **React Big Calendar**: For calendar functionality. Ensure the inclusion of drag-and-drop features (reference [drag-and-drop example](https://codesandbox.io/p/sandbox/react-big-calendar-drag-and-drop-events-between-months-upekt)).
   - **Tailwind CSS**: Ensure seamless dark mode support using [ShadCN’s dark mode setup](https://ui.shadcn.com/docs/dark-mode/next).

2. **Component Specifications**:
   - Build a fully interactive and responsive `schedule.tsx` component.
   - Support adding, updating, and deleting calendar entries.
   - Ensure the UI toggles between dark and light themes based on Tailwind CSS classes.
   - **Dummy Data**: On page reload, default dummy data should be displayed as provided.

3. **Reference & Design**:
   - The design and functionality should exactly replicate the scheduler shown in the shared video

4. **Redux Setup**:
   - The Redux store should manage calendar events, including adding, updating, and deleting events. Set up the store using the provided example with `calendarSlice`.

5. **Data Handling**:
   - Load the provided dummy data on initial load. This data should be displayed by default when the application is opened or refreshed.

6. **Drag-and-Drop Functionality**:
   - Implement drag-and-drop for scheduling events within the calendar. This is a critical feature and must be fully functional.

7. **Code Structure**:
   - Follow the provided Redux setup for managing calendar events. Ensure that the code is modular and easy to maintain.
### 👉🏽 Helper Files & Code Snippets:

#### Redux Setup 

`client/store/redux/store.ts`
```ts

import calendarSlice from "@/components/widgets/workout-schedule-card/slice/calendar-slice";
import { configureStore } from "@reduxjs/toolkit";
import userReducer from "@/features/user-slice";
import mealSlice from "@/components/widgets/meal-plan/slice/food-slice";

const store = configureStore({
  reducer: {
    calendar: calendarSlice,
  },
});

export default store;

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

`client\store\redux\use-selector.ts`
```ts
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";
import { RootState, AppDispatch } from "./store";
export const useAppDispatch: () => AppDispatch = useDispatch;
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

#### Redux Use

`Dummy Data/Initial State` - when reload default this data should show 

### Dummy Data/Initial State
```ts
export const calendar_data: CalendarEvent[] = [
  {
    id: "user1",
    exerciseName: "Bicep & Tricep",
    trainerName: "Raj",
    startTime: "13:12",
    endTime: "14:12",
    period: "PM",
    customerName: "John",
    currentDate: "2024-07-08T05:42:09.599Z",
    userPicture: "/default-user-1.jpg",
  },
  {
    id: "user2",
    exerciseName: "Pullups",
    trainerName: "David",
    startTime: "04:15",
    endTime: "07:15",
    period: "PM",
    customerName: "Raj",
    currentDate: "2024-07-17T05:42:50.207Z",
    userPicture: "/default-user-2.jpg",
  },
  {
    id: "user3",
    exerciseName: "Stretching",
    trainerName: "Kane",
    startTime: "15:22",
    endTime: "16:22",
    period: "PM",
    customerName: "Raj",
    currentDate: "2024-07-22T05:45:42.272Z",
    userPicture: "/default-user-3.jpg",
  },
];
```

### calendar-slice.ts
```ts
import { createSlice, PayloadAction } from "@reduxjs/toolkit";
import { CalendarEvent, CalendarState } from "../types/workout-schedule-card.types";
import { calendar_data } from "@/data/placeholder-data";

const initialState: CalendarState = {
    events: calendar_data,
    // loading: false,
    // error: null,
    // currentDate: new Date().toISOString(),
    currentEvent: null,
};

const calendarSlice = createSlice({
    name: 'calendar',
    initialState,
    reducers: {
        setEvents(state, action: PayloadAction<CalendarEvent[]>) {
            state.events = action.payload;
        },
        addEvent(state, action: PayloadAction<CalendarEvent>) {
            state.events.push(action.payload);
        },
        deleteEvent(state, action: PayloadAction<string>) {
            state.events = state.events.filter(event => event.id !== action.payload);
        },
        updateEvent(state, action: PayloadAction<CalendarEvent>) {
            const index = state.events.findIndex(event => event.id === action.payload.id);
            if (index !== -1) {
                state.events[index] = action.payload;
            }
        },
    },
});

export const {
    addEvent,
    deleteEvent,
    updateEvent,
    setEvents,
} = calendarSlice.actions;

export default calendarSlice.reducer;
```

### types
```ts
import type { Dayjs } from "dayjs";
import dayjs from "dayjs";

export interface Note {
  date: Dayjs;
  content: {
    exercise: string;
    name: string;
    startTime: string;
    endTime: string;
    period: string;
    customerName: string;
    userPicture?: string;
  };
}

export interface CalendarEvent {
  id: string;
  exerciseName: string;
  trainerName: string;
  startTime: string;
  endTime: string;
  period: string;
  customerName: string;
  currentDate: string;
  userPicture?: string;
}

export interface CalendarState {
  events: CalendarEvent[];
  // loading: boolean;
  // error: string | null;
  currentEvent: CalendarEvent | null;
}
```

