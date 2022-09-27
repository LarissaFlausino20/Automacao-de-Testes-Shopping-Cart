# Automa-o-de-Testes-Shopping-Cart
Testes de Qualidade de Software Automatizado na IDE Eclipse



package metodos;

import static org.junit.Assert.assertTrue;

import org.openqa.selenium.Alert;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class Metodos {

	WebDriver driver;

	String msgValorTotal;

	public void iniciarTeste() {
		System.setProperty("webdriver.chrome.driver", "./Drivers/chromedriver.exe");
		driver = new ChromeDriver();
		driver.get("https://react-shopping-cart-67954.firebaseapp.com");
		driver.manage().window().maximize();

	}

	public void clicarPorTexto(String atributo, String size) {
		driver.findElement(By.xpath("//span[text()='" + size + "']")).click();
	}

	public void clicarPorPosicaoProduto(String posicao) {
		driver.findElement(By.xpath("//div[@class='sc-uhudcz-0 iZZGui']/div[" + posicao + "]/button")).click();
	}

	public double valorUnitario(int qtd) {

		String valorCapturado = driver.findElement(By.xpath("//div[@class='sc-11uohgb-4 bnZqjD']/p")).getText();
		String v = valorCapturado.replace("$", "");
		System.out.println(valorCapturado);
		double valor = Double.parseDouble(v);
		System.out.println(valor);

		double valorTotal = valor;
		System.out.println(valorTotal);
		String str = Double.toString(valor);
		this.msgValorTotal = str;

		return valor;

	}

	public void validarCompra(int qtd) {
		Alert alert = driver.switchTo().alert();
		String msgCheckout = alert.getText();
		assertTrue(msgCheckout.contains(this.msgValorTotal));
		driver.quit();
	}

}





package testes;

import org.junit.Before;
import org.junit.Test;

import metodos.Metodos;

public class CarrinhoCompras {

	Metodos metodos = new Metodos(); // ponteiro

	@Before
	public void acessarSite() {
		metodos.iniciarTeste();

	}

	@Test
	public void validarCheckout() throws InterruptedException {
		System.out.println("eu sou automatizador de teste");
		metodos.clicarPorTexto("span", "XXL");
		Thread.sleep(3000);
		metodos.clicarPorPosicaoProduto("1");
		Thread.sleep(3000);
		metodos.valorUnitario(1);
		Thread.sleep(5000);
		metodos.clicarPorTexto("button", "Checkout");
		Thread.sleep(5000);
		metodos.validarCompra(0);

	}
