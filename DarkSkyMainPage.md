package framework.DarkSkyPages;

import org.openqa.selenium.By;
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebElement;
import stepdefinition.SharedSD;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;


public class DarkSkyMainPage extends BasePage {

    private By tempsLine = By.xpath("//div[@class='temps']//span//span");
    private By expandTimeline = By.xpath("//a[1]//span[2]//span[2]");
    private By minTemp = By.xpath("//a[@class='day revealed']//span[@class='minTemp']");
    private By maxTemp = By.xpath("//a[@class='day revealed']//span[@class='maxTemp']");
    private By highTemp = By.xpath("//div[@class='dayDetails revealed']//span[@class='highTemp swip']//span[@class='temp']");
    private By lowTemp = By.xpath("//div[@class='dayDetails revealed']//span[@class='lowTemp swap']//span[@class='temp']");
    private By currentTemp = By.xpath("//span[@class='summary swap']");


    public void clickExpandTimeline()throws InterruptedException{scrollIntoView(expandTimeline);}
    public String getMinTemp(){return getTextFromElement(minTemp);}
    public String getMaxTemp(){return getTextFromElement(maxTemp);}
    public String getHighTemp(){return getTextFromElement(highTemp);}
    public String getLowTemp(){return getTextFromElement(lowTemp);}

    public void verifyTempLine(){

        String getCurrentTemp = getTextFromElement(currentTemp).substring(0, 1);
        int intTemp = Integer.parseInt(getCurrentTemp);

        List<WebElement> tempLine = SharedSD.getDriver().findElements(tempsLine);
        ArrayList<String> getLineTemp = new ArrayList<>();
        for (WebElement element : tempLine){
            getLineTemp.add(element.getText().replace("°", ""));
        }
        ArrayList<Integer> actualTempLine = new ArrayList<>();
        for (String element : getLineTemp){
            actualTempLine.add(Integer.parseInt(element));
            Collections.sort(actualTempLine);
        }

        int minTemp = actualTempLine.get(0);
        int maxTemp = actualTempLine.get(actualTempLine.size()-1);

        if (intTemp>=minTemp && intTemp<=maxTemp){
            return;
        }
    }

}
