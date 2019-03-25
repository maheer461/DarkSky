package framework.DarkSkyPages;

import com.google.common.base.Function;
import org.openqa.selenium.*;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.FluentWait;
import org.openqa.selenium.support.ui.Select;
import org.openqa.selenium.support.ui.Wait;
import stepdefinition.SharedSD;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.TimeUnit;

public class BasePage {

	// This is the most common wait function used in selenium
	public static WebElement webAction(final By locator) {
		Wait<WebDriver> wait = new FluentWait<WebDriver>(SharedSD.getDriver())
				.withTimeout(15, TimeUnit.SECONDS)
				.pollingEvery(1, TimeUnit.SECONDS)
				.ignoring(NoSuchElementException.class)
				.ignoring(StaleElementReferenceException.class)
				.ignoring(ElementClickInterceptedException.class)
				.withMessage(
						"Webdriver waited for 15 seconds but still could not find the element therefore Timeout Exception has been thrown");

		WebElement element = wait.until(new Function<WebDriver, WebElement>() {
			public WebElement apply(WebDriver driver) {
				return driver.findElement(locator);
			}
		});

		return element;
	}

	public void scrollIntoView(By locator){
        WebElement element = SharedSD.getDriver().findElement(locator);
        ((JavascriptExecutor) SharedSD.getDriver()).executeScript("arguments[0].scrollIntoView(true);", element);
        element.click();
    }

	public void clickOn(By locator) {
		webAction(locator).click();
	}

	public void setValue(By locator, String value) {
		webAction(locator).sendKeys(value);
	}

	public String getTextFromElement(By locator) {
		return webAction(locator).getText();
	}

	public boolean isElementDisplayed(By locator) {
		return webAction(locator).isDisplayed();
	}

	public boolean isElementSelected(By locator) {
		return webAction(locator).isSelected();
	}

	public boolean isElementEnabled(By locator){return webAction(locator).isEnabled();}

	public void selectFromDropdown(By locator, String dropdownText) {
		WebElement month = webAction(locator);
		Select selectMonth = new Select(month);
		//select element by visible text
		selectMonth.selectByVisibleText(dropdownText);
	}

	public void selectFromDropdown(By locator, int index) {
		WebElement month = webAction(locator);
		Select selectMonth = new Select(month);
		//select element by index
		selectMonth.selectByIndex(index);
	}

	public void autoComplete(By locator, String expectedCity){
		List<WebElement> cities = SharedSD.getDriver().findElements(locator);
		for (WebElement element : cities){
			if (element.getText().contains(expectedCity)){
				element.click();
				break;
			}
		}
	}

	public void mouseOver(By locator){
		SharedSD.getDriver().manage().window().maximize();
		Actions actions = new Actions(SharedSD.getDriver());
		WebElement element = webAction(locator);
		actions.moveToElement(element).build().perform();
		//webAction(locator).click();
	}

	public void switchToWindow(int index){
		List<String> listOfWindows = new ArrayList<String>(SharedSD.getDriver().getWindowHandles());
		SharedSD.getDriver().switchTo().window(listOfWindows.get(index));
	}

	public void switchToRootWindowAndCloseAllOtherWindows(){
		List<String> listOfWindows = new ArrayList<String>(SharedSD.getDriver().getWindowHandles());
		for (int i=1; i < listOfWindows.size(); i++){
			SharedSD.getDriver().switchTo().window(listOfWindows.get(i));
			SharedSD.getDriver().close();
		}
		SharedSD.getDriver().switchTo().window(listOfWindows.get(0));
	}

	public void alertDismiss(){
		SharedSD.getDriver().switchTo().alert().dismiss();
	}

	public void alertAccept(){
		SharedSD.getDriver().switchTo().alert().accept();
	}

	public void alertGetText(){
		SharedSD.getDriver().switchTo().alert().getText();
	}

	public void alertSend(String Text){
		SharedSD.getDriver().switchTo().alert().sendKeys(Text);
	}

	public static List<WebElement> webActions(final By locator) {
		Wait<WebDriver> wait = new FluentWait<WebDriver>(SharedSD.getDriver())
				.withTimeout(15, TimeUnit.SECONDS)
				.pollingEvery(1, TimeUnit.SECONDS)
				.ignoring(NoSuchElementException.class)
				.ignoring(StaleElementReferenceException.class)
				.ignoring(NotFoundException.class)
				.withMessage("Webdriver waited for 15 seconds but still could not find the element therefore Timeout Exception has been thrown");

		List<WebElement> elements = wait.until(new Function<WebDriver, List<WebElement>>() {
			public List<WebElement> apply(WebDriver driver) {
				return driver.findElements(locator);
			}
		});
		return elements;
	}

}
