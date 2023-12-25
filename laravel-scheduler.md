In Laravel, you can use the task scheduling feature to automate the execution of certain tasks at specified intervals. Laravel's task scheduler is built on top of the cron daemon, making it easy to schedule tasks to run hourly, daily, weekly, or at custom intervals.

## Creating a custom Artisan command

Create a new Artisan command: ```php artisan make:command DailyMessage```. This creates a new subdirectory inside the path app/Console/Commands/DailyMessage.php:

```ruby
...

class DailyMessage extends Command
{
   protected $signature = 'message:daily'; // update this

   protected $description = 'Artisan command to send daily messages'; // update this

   public function __construct()
   {
      parent::_construct();
   }

   public function handle()
   {
      echo 'This is my first basic scheduler';
   }
}
```

Here's a basic overview of how you can use the task scheduler in Laravel:

## Defining Scheduled Tasks

Scheduled tasks are defined in the `App\Console\Kernel` class. This class contains a `schedule` method where you can define all of your scheduled tasks. You can access this file at `app/Console/Kernel.php`.

   ```php
   // app/Console/Kernel.php

   protected function schedule(Schedule $schedule)
   {
      // updated this 
       $schedule->command('message:daily')
            ->everyMinute()
            ->appendOutputTo('scheduler.log'); 
   }

   // this method below is manually created to ensure consistency in timezones
   public function scheduleTimezone()
   {
      return 'America/Chicago';
   }
   ```
You can schedule Artisan commands to run at specific intervals. For example, to run the `my:command` command every day at midnight, you can use the `daily` method:

   ```php
   $schedule->command('my:command')->daily();
   ```

You can replace `'my:command'` with the actual Artisan command you want to schedule. Therefore in this case, we used the custom command we created manually above.

### Common Schedule Frequencies:**
   Laravel provides convenient methods for common schedule frequencies. Here are some examples:

   - `$schedule->everyMinute();` - Run every minute.
   - `$schedule->everyFiveMinutes();` - Run every 5 minutes.
   - `$schedule->everyThirtyMinutes();` - Run every 30 minutes.
   - `$schedule->hourly();` - Run every hour.
   - `$schedule->daily();` - Run the task daily.
   - `$schedule->weekly();` - Run the task weekly.
   - `$schedule->monthly();` - Run the task monthly.
   - `$schedule->dailyAt('22:00');` - Run daily at specific time.
   - `$schedule->twiceDaily(1, 14);` - Run twice a day.
   - `$schedule->weeklyOn(5, '12:00');` - Run once a week on Friday at 12pm.
   - `$schedule->quarterly();` - Run every quarter.
   - `$schedule->yearly();` - Run every year at midnight on the first of January.
   - `$schedule->between('14:00', '22:00');` - Limit the task to between the given times.
   - `$schedule->weekdays();` - Limit to weekdays.
   - `$schedule->sundays();` - Limit to Sundays.

### Custom Schedule Frequencies
   You can also define custom schedules using the `cron` method. For example, to run a task every weekday at 8 AM, you can use:

   ```php
   $schedule->command('my:command')->cron('0 8 * * 1-5');
   ```

   This uses the cron expression `'0 8 * * 1-5'`.

5. **Task Output:**
   You can redirect the output of your scheduled tasks to a file by using the `sendOutputTo` or `appendOutputTo` or `emailOutputTo` methods. For example:

   ```php
   $schedule->command('my:command')->daily()->sendOutputTo(storage_path('logs/my-command.log'));
   ```

### Running the Scheduler

If you want to test it quickly in development server in your local machine, you can just run this command: ```php artisan schedule:work```.

Laravel's task scheduler needs to be executed periodically to dispatch the scheduled tasks. You can achieve this by adding the following Cron entry to your server:

   ```sh
   * * * * * cd /path-to-your-project && php artisan schedule:run >> /dev/null 2>&1
   ```

This Cron entry runs the Laravel task scheduler every minute. 

NB: Remember to replace `'my:command'` with the actual command you want to run, and adjust the scheduling frequency according to your requirements.

By following these steps, you can leverage Laravel's task scheduler to automate various tasks within your application.
