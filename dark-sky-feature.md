@regression @dark-sky
  Feature: Dark-Sky feature

    Background:
      Given I am on DarkSky home page

    @dark-sky-1
    Scenario: Verify timeline is displayed in correct format
      Then I verify timeline is displayed with two hours incremented


    @dark-sky-2
    Scenario: Verify individual day temp timeline
      When I expand todays timeline
      Then I verify lowest and highest temp is displayed correctly


    @dark-sky-3
    Scenario: Verify Current Temperature should not be greater or less than the Temperature from Daily Timeline
      Then I verify current temp is not greater or less then temps from daily timeline
