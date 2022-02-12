# angular-date-pipe
This readme shows how to create a date pipe that simulates how to display a date ago pipe.

## How to use
1. Create folder to store pipes. In the app folder you can create a pipes folder.
2. Run command: ```ng g p DateAgo```
3. Copy code below into your new pipe.
4. In your code, you can now use the pipe: '''<p>Published: {{dateCreated | dateAgo}}</p>'''

## Code

```
import { DatePipe } from '@angular/common';
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'dateAgo',
  pure: true
})
export class DateAgoPipe implements PipeTransform {
  constructor(private datePipe: DatePipe) {
  }

  transform(value: any, args?: any): unknown {
    if (value) {
      const seconds = Math.floor((+new Date() - +new Date(value)) / 1000);

      // less than 30 seconds ago will show as 'Just now'
      if (seconds < 29) {
        return 'Just now';
      }

      if (seconds > 31536000) {
        return this.datePipe.transform(new Date(value));
      }

      if (seconds > 3600) {
        return this.datePipe.transform(new Date(value), "MMM d");
      }

      const intervals: { [key: string]: number } = {
        'h': 3600,
        'm': 60,
        's': 1
      };

      let counter;

      for (const i in intervals) {

        counter = Math.floor(seconds / intervals[i]);

        if (counter > 0) {
          return counter + i // singular (1 day ago)
        }
      }
    }

    return value;
  }
}

```
