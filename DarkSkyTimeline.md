package framework.DarkSkyPages;

import framework.BasePage;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import stepdefinition.SharedSD;

import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.Date;
import java.util.List;

public class DarkSkyTimeline extends BasePage{

    private By timeLine = By.xpath("//div[@class='hours']//span//span");

    public String timeFormat, hourFormat;

    public List getTimeLineHour(){
        List<WebElement> list = SharedSD.getDriver().findElements(timeLine);
        ArrayList<String> timeLineHours = new ArrayList<>();
        for (int i=0; i<list.size(); i++){
            timeLineHours.add(list.get(i).getText());
        }
        return timeLineHours;
    }

    public List getCurrentHour(){

        ArrayList<String> expectedCurrentHour = new ArrayList<>();
        expectedCurrentHour.add(0, "Now");

        Calendar calendar = Calendar.getInstance();
        calendar.setTime(new Date());
        SimpleDateFormat sdf = new SimpleDateFormat("ha");
        for (int i=0; i<11; i++){
            calendar.add(Calendar.HOUR, 2);
            timeFormat = sdf.format(calendar.getTime());
            hourFormat = timeFormat.replace("AM", "am").replace("PM","pm");
            expectedCurrentHour.add(hourFormat);
        }
        return expectedCurrentHour;
    }

}
