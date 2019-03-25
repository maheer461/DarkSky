package stepdefinition;

import cucumber.api.java.en.Given;
import cucumber.api.java.en.Then;
import cucumber.api.java.en.When;
import framework.DarkSkyPages.DarkSkyMainPage;
import framework.DarkSkyPages.DarkSkyTimeline;
import org.testng.Assert;

public class DarkSkySD {

    private DarkSkyMainPage darkSkyMainPage = new DarkSkyMainPage();
    private DarkSkyTimeline darkSkyTimeline = new DarkSkyTimeline();
    private String DarkSkyURL = "https://darksky.net/forecast/40.7127,-74.0059/us12/en";

    @Given("^I am on DarkSky home page$")
    public void verifyHomePage(){
        Assert.assertEquals(SharedSD.getDriver().getCurrentUrl(), DarkSkyURL);}

    @Then("^I verify timeline is displayed with two hours incremented$")
    public void verifyTimeline(){
        Assert.assertEquals(darkSkyTimeline.getTimeLineHour(), darkSkyTimeline.getCurrentHour());
    }

    @When("^I expand todays timeline$")
    public void expandTimeline()throws Exception{darkSkyMainPage.clickExpandTimeline(); Thread.sleep(3000);}

    @Then("^I verify lowest and highest temp is displayed correctly$")
    public void verifyTemp(){
        Assert.assertEquals(darkSkyMainPage.getMaxTemp(), darkSkyMainPage.getHighTemp());
        Assert.assertEquals(darkSkyMainPage.getMinTemp(), darkSkyMainPage.getLowTemp());
    }

    @Then("^I verify current temp is not greater or less then temps from daily timeline$")
    public void verifyCurrentTemp(){darkSkyMainPage.verifyTempLine();}
