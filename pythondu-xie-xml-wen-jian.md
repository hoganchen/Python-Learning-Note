### Python读写xml文件

* ###### 读取xml

```python
def run_test(xml_file):
    xml_tree = xmlET.ElementTree(file=xml_file)
    logging.debug(xml_tree)

    tree_root = xml_tree.getroot()
    logging.debug('tree_root.tag: {}, tree_root.attrib: {}, tree_root.text: {}'.
                  format(tree_root.tag, tree_root.attrib, tree_root.text))

    for child_of_root in tree_root:
        logging.debug('child_of_root.tag: {}, child_of_root.attrib: {}, child_of_root.text: {}'.
                      format(child_of_root.tag, child_of_root.attrib, child_of_root.text))

        for grand_of_root in child_of_root:
            logging.debug('grand_of_root.tag: {}, grand_of_root.attrib: {}, grand_of_root.text: {}'.
                          format(grand_of_root.tag, grand_of_root.attrib, grand_of_root.text))

    rand_elem = tree_root.find('RandomSeed')

    if rand_elem is not None:
        logging.debug('rand_elem.tag: {}, rand_elem.attrib: {}, rand_elem.text: {}'.
                      format(rand_elem.tag, rand_elem.attrib, rand_elem.text))

        random_seed = int(rand_elem.text)
    else:
        random_seed = random.randint(0x01000000, 0xFFFFFFFF)

    for case_elem in tree_root.iter(tag='TestCase'):
        logging.debug('case_elem.tag: {}, case_elem.attrib: {}, case_elem.text: {}'.
                      format(case_elem.tag, case_elem.attrib, case_elem.text))

        logging.debug('Case Name: {}'.format(case_elem.attrib['name']))
        logging.debug('Case Name: {}'.format(case_elem.attrib.get('name')))

        random.seed(random_seed)
```

* ###### 复制xml

```
def gen_failed_xml_files():
    failed_xml_file_list = []

    failed_case_dict = {'ble-tool-test-case_3454520676': ['periodic_adv_channel_test_22', 'periodic_adv_data_test_11',
                                                          'periodic_adv_phy_test_20'],
                        'ble-tool-test-case_4122556646': ['legacy_adv_test_015', 'periodic_adv_data_test_03'],
                        'ble-tool-test-case_558967663': ['periodic_adv_data_test_02', 'periodic_adv_data_test_10',
                                                         'periodic_adv_phy_test_02']
                        }

    for case_info in failed_case_dict.keys():
        match_obj = re.match('^(.*?)_(.*?)$', case_info)
        module_name = match_obj.group(1).replace('-', '_')
        random_seed = match_obj.group(2)
        failed_case_list = failed_case_dict[case_info]

        xml_tree = xmlET.ElementTree(file='{}{}'.format(module_name, test_config.TEST_TEST_CASE_SUFFIX))
        tree_root = xml_tree.getroot()

        failed_xml_et = xmlET.Element('TestCaseConfig')
        failed_xml_et.text = '\n    '

        # random_seed_et = xmlET.Element('RandomSeed')
        # random_seed_et.text = random_seed
        # failed_xml_et.append(random_seed_et)

        # xmlET.SubElement(failed_xml_et, 'RandomSeed').text = random_seed

        random_seed_et = xmlET.SubElement(failed_xml_et, 'RandomSeed')
        random_seed_et.text = random_seed
        random_seed_et.tail = '\n\n    '

        for failed_case in failed_case_list:
            for case_elem in tree_root.iter(tag='TestCase'):
                if case_elem.attrib['name'] == failed_case:
                    # logging.debug(xmlET.dump(case_elem))
                    failed_xml_et.append(case_elem)

        failed_xml_tree = xmlET.ElementTree(element=failed_xml_et)
        failed_xml_file = '{}_rerun_{}{}'.format(module_name, random_seed, test_config.TEST_TEST_CASE_SUFFIX)
        failed_xml_tree.write(failed_xml_file)
        failed_xml_file_list.append(failed_xml_file)

    return failed_xml_file_list
```

* ###### 



