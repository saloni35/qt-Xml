#include <QApplication>
#include <QtXml>
#include <QtXmlPatterns>
#include <QCheckBox>
#include <QPushButton>
#include <QMainWindow>
#include <QVBoxLayout>
#include <QComboBox>

void createGui(QDomNodeList nodeList, QVBoxLayout* layout)
{
    for(int i = 0; i < nodeList.size(); i++)
    {
        QDomNode node = nodeList.at(i);
        QDomElement element = node.toElement();
        if(element.hasAttribute("Name"))
        {
            QString cvarname = element.attribute("Name");
            QString id = element.attribute("ID");
            if(id == "1")
            {
                QCheckBox *box = new QCheckBox;
                layout->addWidget(box);
                box->setText(element.elementsByTagName("text").at(0).firstChild().toText().data());
                box->setChecked(element.elementsByTagName("checkstate").at(0).firstChild().toText().data() == "true" ? true:false);
                QPalette bpalette = box->palette();
                QColor col(element.elementsByTagName("colour").at(0).firstChild().toText().data());
                bpalette.setColor(QPalette::Button, col);
                box->setAutoFillBackground(true);
                box->setPalette(bpalette);
                box->update();

            }
            else if(id == "2")
            {
                QPushButton *button = new QPushButton;
                layout->addWidget(button);
                button->setText(element.elementsByTagName("text").at(0).firstChild().toText().data());
                QPalette bpalette = button->palette();
                QColor col(element.elementsByTagName("colour").at(0).firstChild().toText().data());
                bpalette.setColor(QPalette::Button, col);
                button->setAutoFillBackground(true);
                button->setPalette(bpalette);
                button->update();

            }
            else if(id == "3")
            {
                QComboBox *comboButton = new QComboBox;
                layout->addWidget(comboButton);
                comboButton->addItems(element.elementsByTagName("datalist").at(0).firstChild().toText().data().split(" "));
                comboButton->setCurrentIndex(element.elementsByTagName("currentIndex").at(0).firstChild().toText().data().toInt());
            }
        }
        if(element.childNodes().size() > 0)
            createGui(element.childNodes(), layout);
    }
}


int main(int argc, char * argv[])
{
    QApplication a(argc, argv);
    QUrl schemaUrl("file:///home/saloni/qt-examples/TestXml/testformat.xsd");
    QXmlSchema schema;
    schema.load(schemaUrl);

    if (schema.isValid()) {
            QFile file("/home/saloni/qt-examples/TestXml/test.xml");
            file.open(QIODevice::ReadOnly);

            QXmlSchemaValidator validator(schema);
            if (validator.validate(&file, QUrl::fromLocalFile(file.fileName())))
                qDebug() << "instance document is valid";
            else
                qDebug() << "instance document is invalid";
    }

    QDomDocument xmlBOM;
    QFile file("/home/saloni/qt-examples/TestXml/test.xml");
    if (!file.open(QIODevice::ReadOnly) || !xmlBOM.setContent(&file, true))
        return 1;

    QDomElement root = xmlBOM.documentElement();
    qDebug() << root.tagName();


    QWidget* widget = new QWidget;
    QVBoxLayout* vlayout = new QVBoxLayout;

    createGui(root.childNodes(), vlayout);

    widget->setLayout(vlayout);
    widget->show();

    return a.exec();
}
